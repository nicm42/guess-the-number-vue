<script setup lang="ts">
import { ref } from 'vue'

//import HelloWorld from './components/HelloWorld.vue'
//import TheWelcome from './components/TheWelcome.vue'
const min = 1
const max = 100
const randomNumber = Math.floor(Math.random() * (max - min + 1)) + min
console.log(randomNumber)

let inputValue = ref<number>(min)
</script>

<template>
  <h1>Guess the Number</h1>
  <p>The computer is thinking of a number between {{ min }} and {{ max }} (inclusive)</p>
  <p>Guess what that number is.</p>

  <form>
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
          if (inputValue < min) {
            inputValue = min
          }
          if (inputValue > max) {
            inputValue = max
          }
        }
      "
    />
    <button type="submit">Guess</button>
  </form>
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
