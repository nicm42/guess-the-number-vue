<script setup lang="ts">
defineProps<{
  guessNumber: number
  inputNumber: number
  heat: string
}>()
</script>

<template>
  <li>
    <span>{{ guessNumber }}</span>
    <span>{{ inputNumber }}</span>
    <span>{{ heat }}</span>
  </li>
</template>

<style scoped></style>
