<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>{{count}}</h2>
    <div class="buttons">
      <button
        v-on:click="clickHandler(button, 2)"
        v-for="button of buttons"
        :key="button"
      >{{button}}</button>
    </div>
  </div>
</template>

  <script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data: () => ({ count: 0, buttons: ["increment", "decrement", "reset"] }),
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    },
    reset() {
      this.count = 0;
    },
    clickHandler(name, amount) {
      this[name](amount);
    }
  }
};
</script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
