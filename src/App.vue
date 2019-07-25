<template>
  <main id="app">
    <header class="app__header">
      <div class="wrapper">
        <div class="current-wrapper">
          <div class="pigImage" :class="pigImage"></div>
          <div class="current-box">
            <p>O meu dinheiro</p>
            <div>
              <ICountUp class="current-value" :endVal="current" @ready="onReady"/>€
            </div>
          </div>
        </div>
        <form>
          <div>
              <div class="form-group">
                <label for="name">Descrição</label>
                <input type="text" v-model="newInputText" />
              </div>

              <div class="form-group">
                <label for="value">Euro</label>
                <input type="number" v-model="newInputValue" />
              </div>

              <div class="form-group">
                <label for="income">Income</label>
                <input type="radio" name="type" id="income" value="income" v-model="picked" />
              </div>
              <div class="form-group">
                <label for="expense">Expense</label>
                <input type="radio" name="type" id="expense" value="expense" v-model="picked" />
              </div>
            <p v-show="isValidationAllowed==false">Campos com erros</p>
          </div>
          <input
            type="submit"
            value="Submeter"
            @click="validate"
            @click.prevent=" validate(), checkType()"
          />
        </form>
      </div>
    </header>
    <section class="wrapper grey">
      <div class="board-wrapper">
        <div class="income">
          <header class="total">
            <h2>
              <i>
                <svg xmlns="http://www.w3.org/2000/svg" width="23" height="27">
                  <path
                    fill="#2FE3A1"
                    d="M10.925 22.457c-1.258-.193-2.208-1.011-2.298-2.052a.566.566 0 0 1 .525-.607c.32-.03.594.203.62.513.04.457.518.848 1.153.998v-2.291c-1.554-.372-2.3-1.087-2.3-2.143 0-1.087.99-1.995 2.3-2.205v-.607a.57.57 0 0 1 .575-.563.57.57 0 0 1 .575.563v.605c1.258.193 2.207 1.01 2.298 2.052a.566.566 0 0 1-.525.607.567.567 0 0 1-.62-.513c-.04-.457-.518-.847-1.153-.998v2.292c1.553.371 2.3 1.085 2.3 2.142 0 1.087-.99 1.995-2.3 2.205v.608a.57.57 0 0 1-.575.562.57.57 0 0 1-.575-.563v-.605zm1.15-1.15c.662-.158 1.149-.578 1.149-1.057 0-.451-.368-.77-1.149-.987v2.045zm-1.15-5.49c-.663.159-1.15.58-1.15 1.058 0 .451.369.77 1.15.987v-2.045zm4.44-9.067h.735a.57.57 0 0 1 .575.563.57.57 0 0 1-.575.562h-.968c2.202 2.33 7.293 8.008 7.293 10.688C22.425 23.215 17.524 27 11.5 27 5.476 27 .575 23.215.575 18.562c0-2.68 5.09-8.357 7.293-10.687H6.9a.57.57 0 0 1-.575-.563.57.57 0 0 1 .575-.562h.734C6.18 5.48 3.96 3.318 3.827 1.84c-.038-.423.08-.798.342-1.087.162-.18.467-.395.997-.395 1.156 0 2.993 1.093 3.881 1.67C9.703 1.115 10.674 0 11.5 0c.826 0 1.797 1.116 2.453 2.027.888-.576 2.725-1.67 3.881-1.67.53 0 .835.216.997.396.261.289.38.664.342 1.086-.132 1.479-2.352 3.642-3.808 4.911zm-1.769 0c1.723-1.371 4.326-3.834 4.43-5.007.015-.165-.037-.22-.058-.246-.004 0-.049-.014-.135-.014-.838 0-2.714 1.095-3.7 1.786a.592.592 0 0 1-.442.096.577.577 0 0 1-.376-.248c-.653-.987-1.527-1.938-1.832-1.993-.276.055-1.15 1.006-1.802 1.993a.584.584 0 0 1-.816.152c-.985-.69-2.86-1.786-3.7-1.786a.342.342 0 0 0-.14.02c-.016.02-.066.075-.052.24.104 1.173 2.708 3.636 4.43 5.007h4.193zm-.041 1.125h-4.11c-2.304 2.36-7.72 8.397-7.72 10.688 0 4.032 4.385 7.312 9.775 7.312 5.39 0 9.775-3.28 9.775-7.313 0-2.29-5.416-8.327-7.72-10.687z"
                  />
                </svg>
              </i>Total de Ganhos
            </h2>
            <p>
              {{totalIncome}}
              <span>€</span>
            </p>
          </header>

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
            <button
              v-if="incomes.length >= 2"
              @click="removeList('incomes')"
              class="app__btn"
            >Delete All</button>
          </div>
        </div>
        <div class="expenses">
          <header class="total">
            <h2>
              <i>
                <svg xmlns="http://www.w3.org/2000/svg" width="23" height="27">
                  <path
                    fill="#f71349"
                    d="M10.925 22.457c-1.258-.193-2.208-1.011-2.298-2.052a.566.566 0 0 1 .525-.607c.32-.03.594.203.62.513.04.457.518.848 1.153.998v-2.291c-1.554-.372-2.3-1.087-2.3-2.143 0-1.087.99-1.995 2.3-2.205v-.607a.57.57 0 0 1 .575-.563.57.57 0 0 1 .575.563v.605c1.258.193 2.207 1.01 2.298 2.052a.566.566 0 0 1-.525.607.567.567 0 0 1-.62-.513c-.04-.457-.518-.847-1.153-.998v2.292c1.553.371 2.3 1.085 2.3 2.142 0 1.087-.99 1.995-2.3 2.205v.608a.57.57 0 0 1-.575.562.57.57 0 0 1-.575-.563v-.605zm1.15-1.15c.662-.158 1.149-.578 1.149-1.057 0-.451-.368-.77-1.149-.987v2.045zm-1.15-5.49c-.663.159-1.15.58-1.15 1.058 0 .451.369.77 1.15.987v-2.045zm4.44-9.067h.735a.57.57 0 0 1 .575.563.57.57 0 0 1-.575.562h-.968c2.202 2.33 7.293 8.008 7.293 10.688C22.425 23.215 17.524 27 11.5 27 5.476 27 .575 23.215.575 18.562c0-2.68 5.09-8.357 7.293-10.687H6.9a.57.57 0 0 1-.575-.563.57.57 0 0 1 .575-.562h.734C6.18 5.48 3.96 3.318 3.827 1.84c-.038-.423.08-.798.342-1.087.162-.18.467-.395.997-.395 1.156 0 2.993 1.093 3.881 1.67C9.703 1.115 10.674 0 11.5 0c.826 0 1.797 1.116 2.453 2.027.888-.576 2.725-1.67 3.881-1.67.53 0 .835.216.997.396.261.289.38.664.342 1.086-.132 1.479-2.352 3.642-3.808 4.911zm-1.769 0c1.723-1.371 4.326-3.834 4.43-5.007.015-.165-.037-.22-.058-.246-.004 0-.049-.014-.135-.014-.838 0-2.714 1.095-3.7 1.786a.592.592 0 0 1-.442.096.577.577 0 0 1-.376-.248c-.653-.987-1.527-1.938-1.832-1.993-.276.055-1.15 1.006-1.802 1.993a.584.584 0 0 1-.816.152c-.985-.69-2.86-1.786-3.7-1.786a.342.342 0 0 0-.14.02c-.016.02-.066.075-.052.24.104 1.173 2.708 3.636 4.43 5.007h4.193zm-.041 1.125h-4.11c-2.304 2.36-7.72 8.397-7.72 10.688 0 4.032 4.385 7.312 9.775 7.312 5.39 0 9.775-3.28 9.775-7.313 0-2.29-5.416-8.327-7.72-10.687z"
                  />
                </svg>
              </i>
              Total de Despesas
            </h2>
            <p>
              {{totalExpenses}}
              <span>€</span>
            </p>
          </header>
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
            <button
              class="app__btn"
              v-if="expenses.length >= 2"
              @click="removeList('expenses')"
            >Delete All</button>
          </div>
        </div>
      </div>
      <button
        class="app__btn"
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
      picked: "income",
      newInputText: "",
      newInputValue: "",
      nextId: 0,

      incomes: [],
      expenses: [],
      isValidationAllowed: null,
      searchTerm: ""
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

      //update income
      return result;
    },
    totalExpenses() {
      //sum expenses + input value
      let result = 0;
      this.expenses.forEach(element => {
        result += parseInt(element.value);
      });
      //update expenses
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
    validate() {
      if (
        this.newInputText == "" ||
        this.newInputValue == "" ||
        parseInt(this.newInputValue) <= 0
      ) {
        this.isValidationAllowed = false;
      } else this.isValidationAllowed = true;
    },
    saveData() {
      const incomesParsed = JSON.stringify(this.incomes);
      localStorage.setItem("incomes", incomesParsed);
      const expensesParsed = JSON.stringify(this.expenses);
      localStorage.setItem("expenses", expensesParsed);
    },

    checkType() {
      //confirm is input is empty
      if (this.isValidationAllowed == false) return;

      //store data in object
      let data = {
        id: this.nextId++,
        name: this.newInputText,
        value: this.newInputValue
      };

      //push to correct array
      this.picked == "income"
        ? this.incomes.push(data)
        : this.expenses.push(data);

      this.saveData();

      //empty form
      this.newInputText = "";
      this.newInputValue = "";
      this.picked = "income";
      this.isValidationAllowed = null;
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