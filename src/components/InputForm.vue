<template>
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
</template>
<script>
export default {
    name: "InputForm",
    props: ["incomes", "expenses"],
 data() {
    return {
      picked: "income",
      newInputText: "",
      newInputValue: "",
      nextId: 0,
      isValidationAllowed: null,
    };
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

      this.$emit('saveDataInput');

      //empty form
      this.newInputText = "";
      this.newInputValue = "";
      this.picked = "income";
      this.isValidationAllowed = null;
    },
}
}
</script>
