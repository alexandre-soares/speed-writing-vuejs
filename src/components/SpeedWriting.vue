<template>
  <div class="speed-writing">
    <h1>Speed Writing Test</h1>
    <div class="timer">Time: {{ this.timer }}</div>
    <div class="container">
      <div class="quote-display" ref="quoteDisplay"></div>
      <textarea
        class="quote-input"
        autofocus
        v-model="quoteInput"
        @input="checkCharacter()"
        @keyup.delete="removeLetterClass()"
      ></textarea>
    </div>
  </div>
  <div class="log">
    <h1>Activity Log</h1>
    <table>
      <tr>
        <th>Test</th>
        <th>Number of Words</th>
        <th>Time</th>
      </tr>
      <tr v-for="(log, index) in logs" :key="index">
        <td>{{ log.test }}</td>
        <td>{{ log.numberWords }}</td>
        <td>{{ log.time }} s</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      API_URL: "http://api.quotable.io/random",
      launchTimer: true,
      quoteInput: null,
      timer: 0,
      index: 1,
      startTime: null,
      logs: [],
      words: null,
      accuracy: 0
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
      this.wordCounter(quote);
      this.$refs.quoteDisplay.innerHTML = "";
      quote.split("").forEach(character => {
        const characterSpan = document.createElement("span");
        characterSpan.innerText = character;
        this.$refs.quoteDisplay.appendChild(characterSpan);
      });
      this.quoteInput = null;
      this.numberTyping = 0;

      if (this.launchTimer) {
        this.startTimer();
      }
    },
    wordCounter(str) {
      return (this.words = str.split(" ").length);
    },
    checkCharacter() {
      this.numberTyping++;
      const arrayQuote = this.$refs.quoteDisplay.querySelectorAll("span");
      const arrayValue = this.quoteInput.split("");

      const index = arrayValue.length - 1;

      let correct = false;

      if (
        index === arrayQuote.length - 1 &&
        arrayValue[index] === arrayQuote[index].innerHTML
      ) {
        correct = true;
      }
      if (arrayValue[index] === arrayQuote[index].innerHTML) {
        arrayQuote[index].classList.add("correct");
        arrayQuote[index].classList.remove("incorrect");
      } else {
        arrayQuote[index].classList.remove("correct");
        arrayQuote[index].classList.add("incorrect");
      }

      if (correct) {
        const result = {
          test: `Test ${this.index++}`,
          numberWords: this.words,
          time: this.timer
        };
        this.logs.push(result);

        this.renderNewQuote();
        this.timer = 0;
      }
    },
    removeLetterClass() {
      const arrayQuote = this.$refs.quoteDisplay.querySelectorAll("span");
      const arrayValue = this.quoteInput.split("");

      const index = arrayValue.length;
      arrayQuote[index].classList.remove("correct");
      arrayQuote[index].classList.remove("incorrect");
    },
    startTimer() {
      this.startTime = new Date();
      console.log(`start time is ${this.startTime}`);
      setInterval(() => {
        this.timer = this.getTimerTime();
      }, 100);
    },
    getTimerTime() {
      return ((new Date() - this.startTime) / 1000).toFixed(3);
    }
  }
};
</script>

<style>
.speed-writing {
  flex: 0 0 50%;
  text-align: center;
  margin: 5rem auto 0;
}

.log {
  flex: 0 0 40%;
  margin: 5rem auto 0;
}

h1 {
  letter-spacing: 2px;
  margin-bottom: 4rem;
  font-size: 4rem;
}

.container {
  height: 70vh;
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

table {
  border-collapse: collapse;
  width: 100%;
  font-size: 1.7rem;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
