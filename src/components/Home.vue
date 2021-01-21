<template>
  <div class="home">
    <h1>Speed Writing Test</h1>
    <div>
      <p>Here is a little exercice to test your writing speed.</p>
      <p>
        If you press the start button, you will have to write a little paragraph
        as fast as possible.
      </p>
      <p>
        Once you finished by pressing the last letter of the paragraph, the
        activity log will get updated with your time and a new paragraph will
        appear.
      </p>
    </div>
    <p>Have fun !</p>
    <button class="btn" @click="startGame()">Start</button>
  </div>
</template>

<script>
export default {
  methods: {
    startGame() {
      this.$emit("clicked");
    }
  }
};
</script>

<style>
.home {
  height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  margin: auto;
  font-size: 1.7rem;
  line-height: 1.6;
}

button {
  padding: 2rem;
  width: 20%;
  margin: 0 auto;
  display: block;
  background-color: #4b49db;
  border: none;
  font-size: 1.6rem;
  color: white;
  font-weight: bold;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: 0.3s ease-in;
}

button:hover {
  background-color: #6d6be4;
}

button:focus {
  outline: none;
}
</style>
