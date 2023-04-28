<template>
  <div class="card" :class="{ disable: isDisable }">
    <div
      @click="onToogleFlipCard"
      class="card_inner"
      :class="{ 'is-flipped': isFlipped }"
    >
      <div class="card_face card_face--front">
        <div class="card_content"></div>
      </div>
      <div class="card_face card_face--back">
        <div
          class="card_content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      require: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisable: false,
      isFlipped: false,
    };
  },
  methods: {
    onToogleFlipCard() {
      if (this.isDisable) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnableMode() {
      this.isDisable = true;
    },
  },
};
</script>
<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  width: 90px;
  height: 120px;
}
.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}
.disable.card_iner {
  cursor: default;
}
.card_inner.is-flipped {
  transform: rotateY(-180deg);
}
.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}
.card_face .card_content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  background-size: 40px 40px;
  height: 100%;
  width: 100%;
}
.card_face--back {
  background: var(--light);
  transform: rotateY(-180deg);
}
.card_face--back .card_content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  height: 100%;
  width: 100%;
}
</style>
