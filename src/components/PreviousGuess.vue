<script setup lang="ts">
defineProps<{
  guessNumber: number
  inputNumber: number
  heat: string
}>()
</script>

<template>
  <li class="history">
    <span class="history-counter">{{ guessNumber }}</span>
    <span class="history-guess">{{ inputNumber }}</span>
    <span class="history-heat" :class="heat.toLowerCase()">{{ heat }}</span>
  </li>
</template>

<style scoped lang="scss">
.history {
  margin-bottom: 2em;
  rotate: x -180deg;

  @media (prefers-reduced-motion: no-preference) {
    animation: shift 1s ease-in-out forwards;
  }
}

@keyframes shift {
  from {
    max-height: 0;
    opacity: 0;
    transform: translateY(-200%);
  }
  to {
    max-height: 1.5em;
    opacity: 1;
    transform: translateY(0);
  }
}

//All the elements in each li
.history-counter {
  display: inline-block;
  width: 1.5em;
  color: $defaultColour;
  text-align: right;

  &::after {
    content: ')';
  }
}

.history-guess {
  display: inline-block;
  margin-left: 1em;
  padding: 0.25em;
  height: $historySize;
  width: $historySize;
  line-height: $historySize;
  border-radius: 50%;
  text-align: center;
  background-color: $historyBackground;
  color: $defaultColour;
}

.history-heat {
  padding-left: 1em;
}

//Colours for guesses
.correct {
  font-weight: bold;
}

.boiling {
  color: $boiling;
}

.hot {
  color: $hot;
}

.warm {
  color: $warm;
}

.cold {
  color: $cold;
}

.freezing {
  color: $freezing;
}
</style>
