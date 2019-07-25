<template>
  <main id="app">
    <input-header
      :expenses="expenses"
      :incomes="incomes"
      :current="current"
      @saveDataInput="saveData"
    ></input-header>
    <section-board
      :expenses="expenses"
      :incomes="incomes"
      @saveDataInput="saveData"
      @expensesResult="expensesResult"
      @incomeResult="incomeResult"
      @removeItem="removeItem"
      @removeList="removeList"
      @removeAll="removeAll"
    ></section-board>
  </main>
</template>

<script>
import InputHeader from "./components/InputHeader";
import SectionBoard from "./components/SectionBoard";

require("@/assets/css/app.css");

export default {
  name: "app",
  components: {
    "input-header": InputHeader,
    "section-board": SectionBoard
  },
  data() {
    return {
      incomes: [],
      expenses: [],
      totalExpenses: 0,
      totalIncome: 0
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
    current() {
      return this.totalIncome - this.totalExpenses;
    }
  },
  methods: {
    saveData() {
      const incomesParsed = JSON.stringify(this.incomes);
      localStorage.setItem("incomes", incomesParsed);
      const expensesParsed = JSON.stringify(this.expenses);
      localStorage.setItem("expenses", expensesParsed);
    },
    incomeResult(result) {
      this.totalIncome = result;
    },
    expensesResult(result) {
      this.totalExpenses = result;
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