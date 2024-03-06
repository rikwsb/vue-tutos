<script setup>

  import { ref } from 'vue'
  import { useToast } from 'vue-toastification'

  const text = ref('')
  const amount = ref(0)

  const toast = useToast()

  const emit = defineEmits(['transactionSubmited'])

  const onSubmit = () => {
    if(!text.value && !amount.value && amount.value == 0) {
      toast.error("Both fields mut be filled")
      return;
    }

    const transactionData = {
      text: text.value,
      amount: parseFloat(amount.value)
    }

    emit('transactionSubmitted', transactionData)

  }
</script>

<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
      <div class="form-control">
        <label for="text">Text</label>
        <input type="text" id="text" placeholder="Enter text..." v-model="text"/>
      </div>
      <div class="form-control">
        <label for="amount"
          >Amount <br />
          (negative - expense, positive - income)
        </label>
        <input
          type="text"
          id="amount"
          placeholder="Enter amount..."
          v-model="amount"
        />
      </div>
      <button class="btn">Add transaction</button>
    </form>
  </template>