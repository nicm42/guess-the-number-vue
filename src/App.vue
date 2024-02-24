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
    inputValue.value = undefined
  }
}
</script>

<template>
  <h1>Guess the Number</h1>
  <p>The computer is thinking of a number between {{ min }} and {{ max }} (inclusive)</p>
  <p>Guess what that number is.</p>

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
    <button type="submit">Guess</button>
  </form>

  <ul v-if="numberOfGuesses > 0">
    <PreviousGuess
      v-for="guesses in guessData"
      :key="guesses.guessNumber"
      :guessNumber="guesses.guessNumber"
      :inputNumber="guesses.inputNumber"
      :heat="guesses.heat"
    />
  </ul>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
