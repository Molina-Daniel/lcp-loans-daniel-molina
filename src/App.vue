<template>
  <div id="app">
    <div>
      <img alt="LCP Loans logo" src="./assets/lcp_logo.png" />
      <p>Personal loans made easy</p>
    </div>
    <LoanApp />
  </div>
</template>

<script>
import LoanApp from "./components/LoanApp.vue";

export default {
  name: "App",
  components: {
    LoanApp,
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background-color: #3f3f3f;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  margin-top: 3rem;
}
</style>
