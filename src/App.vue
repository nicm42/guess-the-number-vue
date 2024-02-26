<script setup lang="ts">
import { ref } from 'vue'
import setHeat from './utils/setHeat'
import PreviousGuess from './components/PreviousGuess.vue'

interface IGuessData {
  guessNumber: number
  inputNumber: number
  heat: string
}

const min = 1
const max = 100
const randomNumber = Math.floor(Math.random() * (max - min + 1)) + min
console.log(randomNumber)

let inputValue = ref<number>()
let numberOfGuesses = 0
let heating: string
let guessData: IGuessData[] = []
let showCongrats = false

function submitGuess() {
  if (inputValue.value) {
    heating = setHeat(inputValue.value, randomNumber)
    numberOfGuesses += 1
    const newGuessData = {
      guessNumber: numberOfGuesses,
      inputNumber: inputValue.value,
      heat: heating
    }
    guessData.push(newGuessData)
    if (heating === 'Correct') {
      showCongrats = true
    }
    inputValue.value = undefined
  }
}
</script>

<template>
  <h1>Guess the Number</h1>
  <p>
    The computer is thinking of a number between <span class="min">{{ min }}</span> and
    <span class="max">{{ max }}</span> (inclusive)
  </p>
  <p class="instruction">Guess what that number is.</p>

  <form @submit.prevent="submitGuess">
    <label for="guess">Type a number</label>
    <input
      type="number"
      inputmode="numeric"
      pattern="[0-9]*"
      id="guess"
      name="guess"
      :min="min"
      :max="max"
      step="1"
      required
      :disabled="showCongrats"
      :aria-disabled="showCongrats"
      v-model="inputValue"
      @input="
        () => {
          if (inputValue) {
            if (inputValue < min) {
              inputValue = min
            }
            if (inputValue > max) {
              inputValue = max
            }
          }
        }
      "
    />
    <button type="submit" :disabled="showCongrats" :aria-disabled="showCongrats">Guess</button>
  </form>

  <div class="congrats" v-show="showCongrats">
    <p>
      Well done! You found the number in {{ numberOfGuesses }}
      {{ numberOfGuesses === 1 ? 'guess' : 'guesses' }}.
    </p>
    <p>Refresh the page to play again.</p>
  </div>

  <ul class="temperature" v-if="numberOfGuesses > 0">
    <PreviousGuess
      v-for="guesses in guessData"
      :key="guesses.guessNumber"
      :guessNumber="guesses.guessNumber"
      :inputNumber="guesses.inputNumber"
      :heat="guesses.heat"
    />
  </ul>
</template>

<style scoped lang="scss">
.min,
.max {
  position: relative;
}

.min::after,
.max::after {
  content: '';
  position: absolute;
  left: -2px;
  bottom: 3px;
  height: 3px;
  background-color: $buttonBorder;
}

.min::after {
  width: 0.5em;
}

.max::after {
  width: 1.5em;
}

.instruction {
  font-style: italic;
}

form {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
  margin-top: 2rem;
}

input {
  padding: 0.4em;
  font-size: $standardFontSize;
}

button {
  margin-left: 0.5em;
  padding: 0.5em 1em;
  border: 1px solid $buttonBorder;
  border-radius: 0.25em;
  background-color: $buttonBackground;
  font-size: $standardFontSize;
  cursor: pointer;

  &:hover,
  &:focus {
    background-color: $buttonHover;
  }

  &:active {
    position: relative;
    left: 2px;
    top: 2px;
    color: $defaultColour;
  }

  &:disabled {
    cursor: auto;

    &:hover {
      background-color: $buttonBackground;
    }
  }
}

.congrats {
  margin-top: 2em;
  font-size: 1.3em;
}

.temperature {
  margin: 0;
  margin-top: 0.5em;
  padding: 0;
  display: inline-block;
  list-style-type: none;
  text-align: left;
  rotate: x 180deg;
}
</style>
