<template>
      <header class="app__header">
      <div class="wrapper">
        <div class="current-wrapper">
          <pig-image :current="current"></pig-image>
          <div class="current-box">
            <p>O meu dinheiro</p>
            <div>
              <ICountUp class="current-value" :endVal="current" />€
            </div>
          </div>
        </div>
        <input-form :expenses="expenses" :incomes="incomes" @saveDataInput="saveDataInput"></input-form>
      </div>
    </header>
</template>
<script>
import ICountUp from "vue-countup-v2";
import Pig from "./Pig"
import InputForm from "./InputForm"

export default {
  name: "InputHeader",
  props: ["incomes", "expenses", "current"],
  components: {
    ICountUp,
    'pig-image': Pig,
    'input-form': InputForm
  },
}
</script>
