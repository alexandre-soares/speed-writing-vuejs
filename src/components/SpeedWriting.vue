<template>
  <div class="speed-writing">
    <h1>Speed Writing Test</h1>
    <div class="timer">0</div>
    <div class="container">
      <div class="quote-display" ref="quoteDisplay"></div>
      <textarea
        class="quote-input"
        autofocus
        v-model="quoteInput"
        @input="checkCharacter()"
      ></textarea>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      API_URL: "http://api.quotable.io/random",
      quoteInput: null
    };
  },
  mounted() {
    this.renderNewQuote();
  },
  methods: {
    getRandomQuote() {
      return fetch(this.API_URL)
        .then(response => response.json())
        .then(data => data.content);
    },
    async renderNewQuote() {
      const quote = await this.getRandomQuote();
      this.$refs.quoteDisplay.innerHTML = "";
      quote.split("").forEach(character => {
        const characterSpan = document.createElement("span");
        characterSpan.innerText = character;
        this.$refs.quoteDisplay.appendChild(characterSpan);
      });
      this.quoteInput = null;
    },
    checkCharacter() {
      console.log(this.quoteInput);
      const arrayQuote = this.$refs.quoteDisplay.querySelectorAll("span");
      const arrayValue = this.quoteInput.split("");

      let correct = true;
      arrayQuote.forEach((characterSpan, index) => {
        const character = arrayValue[index];
        if (character == null) {
          characterSpan.classList.remove("correct");
          characterSpan.classList.remove("incorrect");
          correct = false;
        } else if (character === characterSpan.innerText) {
          characterSpan.classList.add("correct");
          characterSpan.classList.remove("incorrect");
        } else {
          characterSpan.classList.remove("correct");
          characterSpan.classList.add("incorrect");
          correct = false;
        }
      });

      if (correct) {
        this.renderNewQuote();
      }
    }
  }
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
  margin: 4rem 0 2rem;
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

.correct {
  color: green;
}

.incorrect {
  color: red;
  text-decoration: line-through;
}
</style>
