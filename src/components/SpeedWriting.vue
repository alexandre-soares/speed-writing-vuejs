<template>
  <div class="speed-writing">
    <h1>Speed Writing Test</h1>
    <div class="timer">0</div>
    <div class="container">
      <div class="quote-display">Quote</div>
      <textarea class="quote-input" autofocus>Quote</textarea>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {}
};
</script>

<style>
h1 {
  letter-spacing: 2px;
  margin-bottom: 4rem;
  font-size: 4rem;
}

.container {
  height: 70vh;
  width: 50vw;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
}

.timer {
  font-size: 4rem;
}

.quote-display {
  font-size: 3rem;
}

.quote-input {
  border: 2px solid black;
  border-radius: 1rem;
  width: 100%;
  height: 15rem;
  padding: 2rem;
  font-size: 1.9rem;
  letter-spacing: 1px;
  line-height: 1.6;
  margin: auto;
  resize: none;
}

.quote-input:focus {
  outline: none;
}
</style>
