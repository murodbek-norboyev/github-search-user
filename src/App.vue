<template>
  <div id="app">
    <div class="container">
      <MainView/>
    </div>
  </div>
</template>

<script>
import MainView from "./components/MainView.vue";

export default {
  name: "App",
  components: {
    MainView,
  },
};
</script>

<style lang="scss">
@import "https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap";
@import "assets/normalize";

body {
  font-family: 'Space Mono', monospace;
  font-weight: 400;
  font-size: 16px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: #F6F8FF;
}

.container {
  max-width: 800px;
  margin: 0 auto;
}
</style>
