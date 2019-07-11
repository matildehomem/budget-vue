<template>
  <main id="app">
    <header>
      <div class="wrapper">
        <div class="current-wrapper">
          <div class="pigImage" :class="pigImage"></div>
          <div class="current-box">
            <p>O meu dinheiro</p>
            <div>
              <ICountUp class="current-value" :endVal="current" />€
            </div>
          </div>
        </div>
        <p>Indique aqui as suas receitas ou despesas</p>
        <form>
          <div>
            <fieldset>
              <label for="name">Descrição</label>
              <input type="text" v-model="newInputText" />
              <label for="value">Euro</label>
              <input type="number" v-model="newInputValue" required />
              <label for="income">Income</label>
              <input type="radio" name="type" value="income" v-model="incomeCheck" />
              <label for="expense">Expense</label>
              <input type="radio" name="type" value="expense" v-model="expenseCheck" />
            </fieldset>
          </div>
          <input type="submit" value="Submeter" @click.prevent="checkType()" />
        </form>
      </div>
    </header>
    <section>
      <div class="wrapper board-wrapper">
        <div class="income">
          <div class="total">
            <h2>Total de Ganhos</h2>
            <p>
              {{totalIncome}}
              <span>€</span>
            </p>
          </div>

          <div class="list">
            <ul>
              <li v-for="(income, index) in incomes" :key="income.id">
                <p>{{income.name}}</p>

                <div>
                  <strong>{{income.value}}€</strong>
                  <button @click="removeItem(index,'incomes')" class="remove"></button>
                </div>
              </li>
            </ul>
            <button v-if="incomes.length >= 2" @click="removeList('incomes')">Delete All</button>
          </div>
        </div>
        <div class="expenses">
          <div class="total">
            <h2>Total de Despesas</h2>
            <p>
              {{totalExpenses}}
              <span>€</span>
            </p>
          </div>
          <div class="list">
            <ul>
              <li v-for="(expense, index) in expenses" v-bind:key="expense.id">
                <p>{{expense.name}}</p>
                <div>
                  <strong>{{expense.value}}€</strong>
                  <button @click="removeItem(index, 'expenses')" class="remove"></button>
                </div>
              </li>
            </ul>
            <button v-if="expenses.length >= 2" @click="removeList('expenses')">Delete All</button>
          </div>
        </div>
      </div>
      <button
        v-if="incomes.length && expenses.length"
        @click="removeAll(['incomes', 'expenses'])"
      >Delete both lists</button>
    </section>
  </main>
</template>

<script>
import ICountUp from "vue-countup-v2";
require("@/assets/css/app.css");
export default {
  name: "app",
  components: {
    ICountUp
  },
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

  mounted() {
    if (localStorage.getItem("incomes")) {
      try {
        this.incomes = JSON.parse(localStorage.getItem("incomes"));
      } catch (e) {
        localStorage.removeItem("incomes");
      }
    }
    if (localStorage.getItem("expenses")) {
      try {
        this.expenses = JSON.parse(localStorage.getItem("expenses"));
      } catch (e) {
        localStorage.removeItem("expenses");
      }
    }
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
    saveData() {
      const incomesParsed = JSON.stringify(this.incomes);
      localStorage.setItem("incomes", incomesParsed);
      const expensesParsed = JSON.stringify(this.expenses);
      localStorage.setItem("expenses", expensesParsed);
    },

    checkType() {
      //if income
      if (this.incomeCheck) {
        //add to income array
        this.incomes.push({
          id: this.nextId++,
          name: this.newInputText,
          value: this.newInputValue
        });

        this.saveData();
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

        this.saveData();

        this.newInputText = "";
        this.newInputValue = "";
        this.expenseCheck = false;
      }
    },
    removeItem(index, list) {
      this[list].splice(index, 1);
      this.saveData();
    },
    removeList(list) {
      this[list] = [];
      this.saveData();
    },
    removeAll(list) {
      list.forEach(this.removeList);
    }
  }
};
</script>