<template>
    <div class="speed-writing">
        <h1>Speed Writing Test</h1>
        <div class="timer">Time: {{ this.timer }}</div>
        <div class="timer">Input Counter: {{ this.inputCounter }}</div>
        <div class="timer">Wrong Counter: {{ this.wrongInputCounter }}</div>
        <div class="timer">
            Accuracy: {{ this.accuracy ? this.accuracy : 0 }}%
        </div>
        <div class="container">
            <div class="quote-display" ref="quoteDisplay"></div>
            <textarea
                class="quote-input"
                autofocus
                v-model="quoteInput"
                @keyup="checkCharacter($event)"
                @keyup.delete="removeLetterClass()"
                ref="quoteInput"
            ></textarea>
        </div>
    </div>
    <div class="log">
        <h1>Activity Log</h1>
        <table>
            <tr>
                <th>Test</th>
                <th>Number of Words</th>
                <th>Accuracy</th>
                <th>Time</th>
            </tr>
            <tr v-for="(log, index) in logs" :key="index">
                <td>{{ log.test }}</td>
                <td>{{ log.numberWords }}</td>
                <td>{{ log.accuracy }} %</td>
                <td>{{ log.time }} s</td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            API_URL: 'https://api.quotable.io/random',
            launchTimer: true,
            quoteInput: '',
            timer: 0,
            index: 1,
            startTime: null,
            logs: [],
            words: null,
            inputCounter: 0,
            wrongInputCounter: 0,
        }
    },
    mounted() {
        this.renderNewQuote()
    },
    computed: {
        accuracy() {
            return Math.floor(
                ((this.inputCounter - this.wrongInputCounter) * 100) /
                    this.inputCounter
            )
        },
    },
    methods: {
        getRandomQuote() {
            return fetch(this.API_URL)
                .then((response) => response.json())
                .then((data) => data.content)
        },
        async renderNewQuote() {
            const quote = await this.getRandomQuote()
            this.wordCounter(quote)
            this.$refs.quoteDisplay.innerHTML = ''
            quote.split('').forEach((character) => {
                const characterSpan = document.createElement('span')
                characterSpan.innerText = character
                this.$refs.quoteDisplay.appendChild(characterSpan)
            })
            this.quoteInput = null
            this.numberTyping = 0

            if (this.launchTimer) {
                this.startTimer()
            }
        },
        wordCounter(str) {
            return (this.words = str.split(' ').length)
        },
        resetCounters() {
            ;(this.timer = 0),
                (this.inputCounter = 0),
                (this.wrongInputCounter = 0)
        },
        checkCharacter(e) {
            console.log('LAUNCH FUNCTION')
            this.inputCounter++

            const arrayQuote = this.$refs.quoteDisplay.querySelectorAll('span')
            const arrayValue = this.quoteInput.split('')
            // CHECK IF USER PRESSED BACKSPACE

            const index = arrayValue.length - 1

            let specialChars = false

            if (
                e.key === 'Backspace' ||
                e.key === 'Shift' ||
                e.key === 'Control' ||
                e.key === 'Alt' ||
                e.key === '' ||
                e.key === 'Meta' ||
                e.key === 'ArrowLeft' ||
                e.key === 'ArrowRight' ||
                e.key === 'ArrowUp' ||
                e.key === 'ArrowDown'
            ) {
                this.inputCounter = this.inputCounter - 1
                specialChars = true
            }

            let correct = false
            arrayQuote.forEach((characterSpan, index) => {
                const character = arrayValue[index]
                if (character == null) {
                    characterSpan.classList.remove('correct')
                    characterSpan.classList.remove('incorrect')
                    // correct = false;
                } else if (character === characterSpan.innerText) {
                    characterSpan.classList.add('correct')
                    characterSpan.classList.remove('incorrect')
                } else {
                    characterSpan.classList.remove('correct')
                    characterSpan.classList.add('incorrect')
                    // correct = false;
                }
            })

            if (specialChars) {
                console.log('special char')
            } else {
                if (arrayValue[index] === arrayQuote[index].innerHTML) {
                    console.log('special char')
                } else {
                    this.wrongInputCounter++
                }
            }

            console.log('before test')
            if (
                index === arrayQuote.length - 1 &&
                arrayValue[index] === arrayQuote[index].innerHTML
            ) {
                console.log('done')
                correct = true
            } else {
                console.log('end but false')
                correct = false
            }

            console.log(`correct is ${correct}`)
            if (correct) {
                console.log('correct')
                const result = {
                    test: `Test ${this.index++}`,
                    numberWords: this.words,
                    time: this.timer,
                    accuracy: Math.floor(
                        ((this.inputCounter - this.wrongInputCounter) * 100) /
                            this.inputCounter
                    ),
                }
                this.logs.push(result)
                this.resetCounters()
                this.renderNewQuote()
            }
        },
        removeLetterClass() {
            const arrayQuote = this.$refs.quoteDisplay.querySelectorAll('span')
            const arrayValue = this.quoteInput.split('')

            const index = arrayValue.length

            arrayQuote[index].classList.remove('correct')
            arrayQuote[index].classList.remove('incorrect')
        },
        startTimer() {
            this.startTime = new Date()
            console.log(`start time is ${this.startTime}`)
            setInterval(() => {
                this.timer = this.getTimerTime()
            }, 100)
        },
        getTimerTime() {
            return ((new Date() - this.startTime) / 1000).toFixed(3)
        },
    },
}
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
