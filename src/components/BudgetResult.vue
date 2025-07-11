<template>
  <div class="budget-results">
    <h2 class="results-title">Your Budget Summary</h2>

    <div class="summary-card">
      <div class="summary-item">
        <span class="summary-label">Total Income:</span>
        <span class="summary-value income">${{ income.toFixed(2) }}</span>
      </div>

      <div class="summary-item">
        <span class="summary-label">Total Expenses:</span>
        <span class="summary-value expense">${{ totalExpenses.toFixed(2) }}</span>
      </div>

      <div class="summary-item">
        <span class="summary-label">Remaining Balance:</span>
        <span class="summary-value" :class="balanceClass">${{ remainingBalance.toFixed(2) }}</span>
      </div>
    </div>

    <div class="expense-breakdown">
      <h3 class="breakdown-title">Expense Breakdown</h3>
      <div class="expense-chart">
        <div v-for="(amount, category) in expenses" :key="category" class="chart-item">
          <div class="chart-category">{{ category }}</div>
          <div class="chart-bar-container">
            <div
              class="chart-bar"
              :style="{ width: getPercentage(amount) + '%' }"
              :class="getBarClass(amount)"
            ></div>
            <span class="chart-value">${{ amount.toFixed(2) }}</span>
          </div>
        </div>
      </div>
    </div>

    <button @click="$emit('reset-form')" class="reset-btn">Start New Budget</button>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps({
  income: { type: Number, required: true },
  expenses: { type: Object as () => Record<string, number>, required: true },
})

const emit = defineEmits(['reset-form'])

const totalExpenses = computed(() => {
  return Object.values(props.expenses).reduce((sum, amount) => sum + amount, 0)
})

const remainingBalance = computed(() => {
  return props.income - totalExpenses.value
})

const balanceClass = computed(() => {
  return remainingBalance.value >= 0 ? 'positive' : 'negative'
})

function getPercentage(amount: number): number {
  if (totalExpenses.value === 0) return 0
  return (amount / totalExpenses.value) * 100
}

function getBarClass(amount: number): string {
  const percentage = getPercentage(amount)
  if (percentage > 50) return 'high'
  if (percentage > 25) return 'medium'
  return 'low'
}
</script>

<style scoped>
.budget-results {
  max-width: 800px;
  margin: 40px auto;
  padding: 24px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  font-family: 'Segoe UI', sans-serif;
}

.results-title {
  font-size: 24px;
  font-weight: 600;
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.summary-card {
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 24px;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 12px;
  font-size: 16px;
}

.summary-label {
  color: #555;
}

.summary-value {
  font-weight: bold;
}

.income {
  color: #28a745;
}

.expense {
  color: #dc3545;
}

.positive {
  color: #007bff;
}

.negative {
  color: #ff0000;
}

.expense-breakdown {
  margin-bottom: 30px;
}

.breakdown-title {
  font-size: 20px;
  font-weight: 500;
  margin-bottom: 16px;
  color: #333;
}

.expense-chart {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.chart-item {
  display: flex;
  flex-direction: column;
}

.chart-category {
  font-weight: 500;
  margin-bottom: 4px;
  color: #444;
}

.chart-bar-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.chart-bar {
  height: 20px;
  border-radius: 4px;
  transition: width 0.3s ease;
}

/* Dynamic Bar Classes */
.high {
  background-color: #dc3545;
}

.medium {
  background-color: #ffc107;
}

.low {
  background-color: #28a745;
}

.chart-value {
  font-size: 14px;
  color: #333;
}

.reset-btn {
  display: block;
  margin: 0 auto;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  font-size: 15px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.reset-btn:hover {
  background-color: #0056b3;
}
</style>
