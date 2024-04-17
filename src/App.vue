<template>
  <div id="app">
    <simon-game />
  </div>
</template>

<script>
import SimonGame from "./components/SimonGame.vue";

export default {
  name: "App",
  components: {
    SimonGame,
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
#app {
  text-align: center;
  color: #2c3e50;
  background: black;
  min-width: 100svw;
  min-height: 100svh;
}
</style>
