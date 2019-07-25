<template>
     <header class="app__header">
      <div class="wrapper">
        <div class="current-wrapper">
          <pig-image :current="current"></pig-image>

          <div class="current-box">
            <p>O meu dinheiro</p>
            <div>
              <ICountUp class="current-value" :endVal="current" @ready="onReady"/>€
            </div>
          </div>
        </div>
        <input-form :incomes="incomes" :expenses="expenses" @saveDataInputDois="saveData"></input-form>
      </div>
    </header>
</template>
<script>
import ICountUp from "vue-countup-v2";
import Pig from "./Pig";
import Form from "./Form";

export default {
  name: "app",
  components: {
    ICountUp,
    "pig-image": Pig,
    "input-form": Form
  },
  props: ["incomes", "expenses", "current"],
  methods:{
    saveData(){
      this.$emit('saveDataInput');
    }
  }
}
</script>