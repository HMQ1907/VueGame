<template>
  <h1>Welcome to my game</h1>
  <MainScreen
    v-if="statusMath === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <InteractScreen
    @onFinish="onGetResult()"
    :cardsContext="setting.cardsContext"
    v-if="statusMath === 'match'"
  />
  <ResultScreen
    v-if="statusMath === 'result'"
    :timer="timer"
    @StartAgain="statusMath == 'default'"
  />
  <p class="copyright">This game owned by Ho Minh Quang in Vue 3</p>
</template>
<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { suffled } from "./utils/array.js";
export default {
  name: "App",
  data() {
    return {
      setting: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMath: "default",
      timer: 0,
    };
  },
  components: {
    MainScreen: MainScreen,
    InteractScreen: InteractScreen,
    ResultScreen: ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.setting.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from(
        { length: this.setting.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.setting.cardsContext = suffled(suffled(suffled(cards)));
      this.setting.startedAt = new Date().getTime();
      console.log(this.setting.cardsContext);
      // data ready
      this.statusMath = "match";
    },
    onGetResult() {
      // get time
      this.timer = new Date().getTime() - this.setting.startedAt;
      this.statusMath = "result";
    },
  },
};
</script>

<style lang="css" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
}
</style>
