<template>
  <div id="app">
    <h1>Budget App - Vue</h1>

    <div class="current-value">
      Valor currente:
      <span>{{current}}</span>
    </div>

    <div class="board-wrapper">
      <div class="income">
        <h2>Income</h2>
        <div class="list">
          <ul>
            <li v-for="(income, index) in incomes" v-bind:key="income.id">
              {{income.name}} -
              <strong>{{income.value}} €</strong>
              <button @click="removeItem(index,'incomes')">delete</button>
            </li>
          </ul>
        </div>
        <div class="total">
          <p>
            TOTAL:
            <span>{{totalIncome}}</span>
          </p>
        </div>
      </div>
      <div class="expenses">
        <h2>Expenses</h2>
        <div class="list">
          <ul>
            <li v-for="(expense, index) in expenses" v-bind:key="expense.id">
              {{expense.name}} -
              <strong>{{expense.value}} €</strong>
              <button @click="removeItem(index, 'expenses')">delete</button>
            </li>
          </ul>
        </div>
        <div class="total">
          <p>
            TOTAL:
            <span>{{totalExpenses}}</span>
          </p>
        </div>
      </div>
    </div>
    <form>
      <label for="name">Descrição</label>
      <input type="text" v-model="newInputText" />
      <label for="value">Euro</label>
      <input type="number" v-model="newInputValue" />
      <label for="income">Income</label>
      <input type="checkbox" name="income" value="income" v-model="incomeCheck" />
      <label for="expense">Expense</label>
      <input type="checkbox" name="expense" value="expense" v-model="expenseCheck" />
      <input type="submit" value="submit" @click.prevent="calCurrent" />
    </form>
  </div>
</template>

<script>
require("@/assets/css/app.css");
export default {
  name: "app",
  data() {
    return {
      
      incomeCheck: false,
      expenseCheck: false,

      newInputText: "",
      newInputValue: "",

      incomes: [],
      expenses: [],

      nextId: 0
    };
  },
  computed: {
    totalIncome() {
      //sum current + input value
      let result = 0;
      this.incomes.forEach(e => {
        result += parseInt(e.value);
      });

      return result;

      //update income
    },
    totalExpenses() {
      //sum expenses + input value
      let result = 0;
      this.expenses.forEach(element => {
        result += parseInt(element.value);
      });

      return result;


    },
   
    current() {
      return this.totalIncome - this.totalExpenses;
    }
  },
  methods: {
    calCurrent() {
      //if income
      if (this.incomeCheck) {
        //add to income array
        this.incomes.push({
          id: this.nextId++,
          name: this.newInputText,
          value: this.newInputValue
        });
        this.newInputText = "";
        this.newInputValue = "";
        this.incomeCheck = false;

      }

      //if expense
      if (this.expenseCheck) {
        //add to income array
        this.expenses.push({
          id: this.nextId++,
          name: this.newInputText,
          value: this.newInputValue
        });
        this.newInputText = "";
        this.newInputValue = "";
        this.expenseCheck = false;

      }
    },
     removeItem(index, list) {
      this[list].splice(index, 1);
     
    },
  }
};
</script>


