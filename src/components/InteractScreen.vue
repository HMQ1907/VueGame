<template>
  <div class="screen">
    <div class="screen_inner">
      <CardFlip
        v-for="(card, index) in cardsContext"
        :key="index"
        :imgBackFaceUrl="`images/${card}.png`"
        :ref="`card-${index}`"
        :card="{ index: index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>
<script>
import CardFlip from "./CardScreen.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value == this.rules[1].value
      ) {
        this.$refs[`card-${this.rules[0].index}`][0].onEnableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableMode();
        this.rules = [];
        const disableElement = document.querySelectorAll(".disable");
        if (
          disableElement &&
          disableElement.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 1000);
        }
        console.log("right");
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value != this.rules[1].value
      ) {
        // close card, reset rules = 0
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
          console.log("wrong");
        }, 1000);
      } else {
        return false;
      }
    },
  },
};
</script>
<style lang="css" scoped>
/* .screen {
  text-align: center;
  width: 100%;
  height: 130vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 10;
  background-color: var(--dark);
  color: var(--light);
}
.screen_inner {
  width: 400px;
  display: flex;
  flex-wrap: wrap;
  margin: 2rem 2rem auto;
} */
</style>
