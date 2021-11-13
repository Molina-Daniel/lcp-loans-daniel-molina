<template>
  <div id="app">
    <!-- <LoanApp /> -->
    <!-- <Slider /> -->
    <!-- <vue-slider v-model="value"></vue-slider> -->
    <LoanApp />
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import LoanApp from "./components/LoanApp.vue";
// import VueSlider from "vue-slider-component";
// import "vue-slider-component/theme/default.css";
// import Slider from "./components/Slider.vue";
// import Slider2 from "./components/Slider2.vue";

export default {
  name: "App",
  components: {
    LoanApp,
    // VueSlider,
    // Slider2,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
