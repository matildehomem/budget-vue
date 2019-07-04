<template>
  <div id="app">
    <header>
      <div class="wrapper">
        <div class="current-wrapper">



        <div class="pigImage" :class="pigImage"></div>
      <div class="current-box">

        <p>O meu dinheiro</p>
        <div class="current-value">{{current}} €</div>
      </div>
        </div>
        <p>Indique aqui as suas receitas ou despesas</p>


        <form>
          <label for="name">Descrição</label>
          <input type="text" v-model="newInputText" />
          <label for="value">Euro</label>
          <input type="number" v-model="newInputValue" required />
          <label for="income">Income</label>
          <input type="radio" name="type" value="income" v-model="incomeCheck" required />
          <label for="expense">Expense</label>
          <input type="radio" name="type" value="expense" v-model="expenseCheck" />
          <input type="submit" value="submit" @click.prevent="calCurrent()" />
        </form>
      </div>
    </header>
    <section>
      <div class="wrapper board-wrapper">
        <div class="income">
          <h2>Income</h2>
          <div class="total">
            <p>
              TOTAL:
              <span>{{totalIncome}} €</span>
            </p>
          </div>
          <div class="list">
            <ul>
              <li v-for="(income, index) in incomes" :key="income.id">
                {{income.name}} -
                <strong>{{income.value}} €</strong>
                <button @click="removeItem(index,'incomes')">delete</button>
              </li>
            </ul>
          </div>
        </div>
        <div class="expenses">
          <h2>Expenses</h2>
          <div class="total">
            <p>
              TOTAL:
              <span>{{totalExpenses}}</span>
            </p>
          </div>
          <div class="list">
            <ul>
              <li v-for="(expense, index) in expenses" v-bind:key="expense.id">
                {{expense.name}} -
                <strong>{{expense.value}} €</strong>
                <button @click="removeItem(index, 'expenses')">delete</button>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>
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
    },
    pigImage: function() {
      if (this.current < 0 && this.current > -200) {
        return "unhappy";
      } else if (this.current > 200) {
        return "happy";
      } else if (this.current < -200) {
        return "dead";
      }
      return "normal";
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
    }
  }
};
</script>


