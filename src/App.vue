<template>
  <div id="app">
    <div>
      <h1>This is a tic tac toe app.</h1>
      <TicTacToe size="3" />
    </div>
  </div>
</template>

<script>
import TicTacToe from "./components/TicTacToe.vue";

export default {
  name: "App",
  components: {
    TicTacToe,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: space-evenly;
}
</style>
