<template>
  <div class="app">
    <Home v-if="!start" @clicked="start = !start" />
    <Speed-writing v-if="start" />
  </div>
</template>

<script>
import Home from "./components/Home.vue";
import SpeedWriting from "./components/SpeedWriting.vue";

export default {
  name: "App",
  components: {
    SpeedWriting,
    Home
  },
  data() {
    return {
      start: false
    };
  }
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.app {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: space-evenly;
  min-height: 100vh;
  width: 90vw;
  font-family: "Roboto", sans-serif;
  margin: auto;
}

@media only screen and (max-width: 700px) {
  .app {
    flex-direction: column;
    margin: 5rem auto;
  }
}

html {
  font-size: 62.5%;
}
</style>
