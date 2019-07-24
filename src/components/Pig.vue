</<template>
    <div class="pigImage" :class="pigImage"></div>
</template>
<script>
export default {
  name: "PigImage",
  props: ["current"],
  computed: {
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
  }
};
</script>