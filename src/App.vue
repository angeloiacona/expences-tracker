<template>
    <div class="app">
      <h1>Expenses Tracker</h1>
  
      <!-- Form for adding expenses -->
      <form @submit.prevent="addExpense" class="expense-form">
        <input
          v-model="newExpense.name"
          type="text"
          placeholder="Expense name"
          required
        />
        <input
          v-model.number="newExpense.amount"
          type="number"
          placeholder="Amount"
          required
          min="0"
        />
        <button type="submit">Add Expense</button>
      </form>
  
      <!-- List of expenses -->
      <ul class="expense-list">
        <li v-for="(expense, index) in expenses" :key="index">
          <span>{{ expense.name }}: ${{ expense.amount.toFixed(2) }}</span>
          <button @click="removeExpense(index)">Remove</button>
        </li>
      </ul>
  
      <!-- Total expenses -->
      <div class="total">
        <strong>Total: ${{ totalAmount.toFixed(2) }}</strong>
      </div>
    </div>
</template>
  
<script lang="ts">
  import { computed, defineComponent, reactive, ref } from "vue";
  
  interface Expense {
    name: string;
    amount: number;
  }
  
  export default defineComponent({
    name: "App",
    setup() {
      // Reactive state for the expense form
      const newExpense = reactive<Expense>({
        name: "",
        amount: 0,
      });
  
      // Reactive state for the list of expenses
      const expenses = ref<Expense[]>([]);
  
      // Computed property to calculate the total amount
      const totalAmount = computed(() =>
        expenses.value.reduce((sum, expense) => sum + expense.amount, 0)
      );
  
      // Method to add a new expense
      const addExpense = (): void => {
        if (newExpense.name.trim() && newExpense.amount > 0) {
          expenses.value.push({ ...newExpense });
          newExpense.name = "";
          newExpense.amount = 0;
        }
      };
  
      // Method to remove an expense
      const removeExpense = (index: number): void => {
        expenses.value.splice(index, 1);
      };
  
      return {
        newExpense,
        expenses,
        totalAmount,
        addExpense,
        removeExpense,
      };
    },
  });
</script>
  
<style>
  .app {
    max-width: 400px;
    margin: 20px auto;
    font-family: Arial, sans-serif;
  }
  
  h1 {
    text-align: center;
  }
  
  .expense-form {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }
  
  .expense-form input {
    flex: 1;
    padding: 8px;
  }
  
  .expense-form button {
    padding: 8px 12px;
  }
  
  .expense-list {
    list-style: none;
    padding: 0;
    margin: 0 0 20px;
  }
  
  .expense-list li {
    display: flex;
    justify-content: space-between;
    padding: 5px 0;
    border-bottom: 1px solid #ccc;
  }
  
  .expense-list button {
    background: red;
    color: white;
    border: none;
    padding: 2px 6px;
    cursor: pointer;
  }
  
  .total {
    text-align: center;
    font-size: 18px;
    font-weight: bold;
  }
</style>
  