<template>
  <form @submit.prevent="submitForm" class="budget-form">
    <h2 class="form-title">Budget Planner</h2>

    <div class="form-group">
      <label class="form-label">Monthly Income</label>
      <div class="input-group">
        <span class="input-icon">$</span>
        <input
          v-model.number="income"
          type="number"
          class="form-input"
          placeholder="0.00"
          min="0"
          step="0.01"
        />
      </div>
    </div>

    <div class="expenses-section">
      <h3 class="section-title">Monthly Expenses</h3>

      <div v-for="(amount, category) in expenses" :key="category" class="expense-item">
        <label class="expense-label">{{ category }}</label>
        <div class="input-group">
          <span class="input-icon">$</span>
          <input
            type="number"
            v-model.number="expenses[category]"
            class="form-input"
            placeholder="0.00"
            min="0"
            step="0.01"
          />
          <button
            type="button"
            @click="removeCategory(category)"
            class="remove-btn"
            aria-label="Remove category"
          >
            &times;
          </button>
        </div>
      </div>

      <button type="button" @click="addCategory" class="add-category-btn">
        <span class="plus-icon">+</span> Add Expense Category
      </button>
    </div>

    <div class="form-actions">
      <button type="submit" class="submit-btn">Calculate Budget</button>
    </div>
  </form>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['update-budget'])

const income = ref(0)
const expenses = ref({
  'Housing/Rent': 0,
  Transportation: 0,
  Utilities: 0,
})

function addCategory() {
  const newCategory = prompt('Enter new category name:')
  if (newCategory && !(newCategory in expenses.value)) {
    expenses.value[newCategory] = 0
  } else if (newCategory) {
    alert('This category already exists!')
  }
}

function removeCategory(category: string) {
  if (confirm(`Remove ${category} category?`)) {
    delete expenses.value[category]
  }
}

function submitForm() {
  emit('update-budget', {
    income: income.value,
    expenses: { ...expenses.value },
  })
}
</script>

<style scoped>
.budget-form {
  max-width: 500px;
  margin: 0 auto;
  padding: 2rem;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.form-title {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 1.5rem;
  font-weight: 600;
}

.section-title {
  color: #2c3e50;
  margin: 1.5rem 0 1rem;
  font-weight: 500;
  font-size: 1.1rem;
}

.form-group {
  margin-bottom: 1.5rem;
}

.expenses-section {
  margin-top: 2rem;
}

.expense-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 1rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #eee;
}

.expense-label {
  flex: 1;
  color: #34495e;
  font-weight: 500;
}

.input-group {
  display: flex;
  align-items: center;
  position: relative;
}

.input-icon {
  position: absolute;
  left: 10px;
  color: #7f8c8d;
  font-weight: 500;
}

.form-input {
  width: 150px;
  padding: 0.6rem 0.6rem 0.6rem 2rem;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.form-input:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 0 2px rgba(52, 152, 219, 0.2);
}

.remove-btn {
  margin-left: 0.5rem;
  background: #e74c3c;
  color: white;
  border: none;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
}

.remove-btn:hover {
  background: #c0392b;
}

.add-category-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  padding: 0.7rem;
  background: #f8f9fa;
  border: 1px dashed #bdc3c7;
  border-radius: 6px;
  color: #7f8c8d;
  cursor: pointer;
  transition: all 0.2s;
  margin-top: 1rem;
}

.add-category-btn:hover {
  background: #e8f4fc;
  border-color: #3498db;
  color: #3498db;
}

.plus-icon {
  font-size: 1.2rem;
  margin-right: 0.5rem;
}

.form-actions {
  margin-top: 2rem;
  text-align: center;
}

.submit-btn {
  background: #3498db;
  color: white;
  border: none;
  padding: 0.8rem 2rem;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s;
  width: 100%;
}

.submit-btn:hover {
  background: #2980b9;
}

@media (max-width: 480px) {
  .budget-form {
    padding: 1.5rem;
  }

  .expense-item {
    flex-direction: column;
    align-items: flex-start;
  }

  .input-group {
    margin-top: 0.5rem;
    width: 100%;
  }

  .form-input {
    width: 100%;
  }
}
</style>
