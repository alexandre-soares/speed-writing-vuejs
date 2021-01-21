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
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: space-evenly;
  min-height: 100vh;
  width: 90vw;
  margin: 5rem auto 0;
  font-family: "Roboto", sans-serif;
}

html {
  font-size: 62.5%;
}
</style>
