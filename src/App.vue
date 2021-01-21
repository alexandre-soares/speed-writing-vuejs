<template>
  <Speed-writing />
</template>

<script>
import SpeedWriting from "./components/SpeedWriting.vue";

export default {
  name: "App",
  components: {
    SpeedWriting
  }
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
}
</style>
