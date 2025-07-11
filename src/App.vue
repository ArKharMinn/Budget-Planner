<template>
  <div class="budget-app">
    <BudgetForm @update-budget="handleSubmit" />
    <BudgetResults v-if="submitted" :income="income" :expenses="expenses" @reset-form="resetForm" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import BudgetForm from './components/BudgetForm.vue'
import BudgetResults from './components/BudgetResult.vue'

const submitted = ref(false)
const income = ref(0)
const expenses = ref({})

function handleSubmit(data: { income: number; expenses: Record<string, number> }) {
  income.value = data.income
  expenses.value = data.expenses
  submitted.value = true
}

function resetForm() {
  submitted.value = false
  income.value = 0
  expenses.value = {}
}
</script>
