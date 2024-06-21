<template>
  <MainScreen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart" />

  <InteractScreen
    v-if="statusMatch === 'match'"
    :cardContext="settings.cardContext"
    @onFinish="onHandleResult"
  />

  <ResultScreen
    v-if="statusMatch === 'result'"
    :time="timer"
    @onPlayAgain="statusMatch = 'default'"
  />
</template>

<script setup>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import ResultScreen from './components/ResultScreen.vue'
import { shuffle } from './utils/array.js'
// import ResultScreen from './components/ResultScreen.vue'

import { ref } from 'vue'
const statusMatch = ref('default')
const timer = ref(0)
const settings = ref({
  blocks: 0,
  time: 0,
  cardContext: [],
})

const onHandleBeforeStart = (config) => {
  settings.value.blocks = config.blocks

  const firstCards = Array.from({ length: settings.value.blocks / 2 }, (_, i) => i + 1)

  const secondCards = [...firstCards]

  const cards = [...firstCards, ...secondCards]

  // Tron mang
  settings.value.cardContext = shuffle(cards)

  // Dem thoi gian
  settings.value.time = new Date().getTime()
  // Bat dau tro choi
  statusMatch.value = 'match'
}

const onHandleResult = () => {
  timer.value = Math.round((new Date().getTime() - settings.value.time) / 1000)
  statusMatch.value = 'result'
}
</script>
