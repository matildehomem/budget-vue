<template>
  <section class="wrapper grey">
    <div class="board-wrapper">
      <incomes
        :incomes="incomes"
        @removeItem="removeItem"
        @removeList="removeList"
        @incomeResult="incomeResult"
      ></incomes>
      <expenses
        :expenses="expenses"
        @removeItem="removeItem"
        @removeList="removeList"
        @expensesResult="expensesResult"
      ></expenses>
    </div>
    <button
      class="app__btn"
      v-if="incomes.length && expenses.length"
      @click.prevent="$emit('removeAll', ['incomes','expenses'])"
    >Delete both lists</button>
  </section>
</template>
<script>
import Incomes from "./Incomes";
import Expenses from "./Expenses";
export default {
  components: {
    incomes: Incomes,
    expenses: Expenses
  },
  props: ["expenses", "incomes"],
  methods: {
    removeItem(index, list) {
      this.$emit('removeItem', index, list);
    },
    removeList(list){
      this.$emit('removeList', list);
    },
    incomeResult(result) {
      this.$emit("incomeResult", result);
    },
    expensesResult(result) {
      this.$emit("expensesResult", result);
    }
  }
};
</script>

