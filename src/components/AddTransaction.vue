<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" v-model="text" placeholder="Enter text..."/>
        </div>
        <div class="form-control">
            <label for="amount"
            >Amount <br />
        (Add "-" for expenses and "+" for income)</label>
        
        <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text = ref('');
const amount = ref('');

const emit = defineEmits(['transactionSubmitted']);

const toast = useToast();

const onSubmit = () => {
// validation
if(!text.value || !amount.value) {
    toast.error('Both fields must be filled');
    return;
}

// replace comma with .dot
const normalizedAmount = amount.value.replace(',', '.'); 
const parsedAmount = parseFloat(normalizedAmount);

// check if parsedAmount is a valid number
if (isNaN(parsedAmount)) {
    toast.error('Amount must be a valid number');
    return;
}

const transactionData = {
    text: text.value,
    amount: parsedAmount
};

emit('transactionSubmitted', transactionData);

        text.value = '';
        amount.value= '';
    };

</script>