<template>
  <div class="result">
    <h1>✨ Congratulations ✨</h1>
    <p>{{ props.time }} seconds</p>
    <button @click="$emit('onPlayAgain')">Play Again</button>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  time: {
    type: Number,
    required: true,
  },
})
</script>

<style lang="css" scoped>
.result {
  background-color: var(--dark);
  height: 100vh;
  color: var(--light);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
h1 {
  font-size: 60px;
  letter-spacing: 1px;
  margin: 0px;
  text-transform: uppercase;
}
p {
  font-size: 38px;
  margin: 20px 0px;
}
button {
  background-color: transparent;
  border: 1px solid var(--light);
  color: var(--light);
  font-size: 18px;
  padding: 14px 25px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
  border-radius: 10px;
}
button:hover {
  background-color: var(--light);
  color: var(--dark);
}
</style>
