<template>
  <div class="main-screen">
    <h1>Poke Memmories</h1>
    <h4>Select mode to start game</h4>
    <div class="mode-game">
      <button
        class="mode-box"
        v-for="item in modeBox"
        :key="item.mode"
        @click="onStart(item.blocks)"
      >
        <span class="mode">{{ item.mode }}</span>
        <span class="name">{{ item.name }}</span>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue'
const emit = defineEmits(['onStart'])

const modeBox = ref([
  {
    mode: '4x4',
    blocks: 16,
    name: 'Easy',
  },
  {
    mode: '6x6',
    blocks: 36,
    name: 'Normal',
  },
  {
    mode: '8x8',
    blocks: 64,
    name: 'Hard',
  },
  {
    mode: '10x10',
    blocks: 100,
    name: 'Super hard',
  },
])

const onStart = (blocks = 16) => {
  emit('onStart', { blocks: blocks })
}
</script>

<style lang="css" scoped>
.main-screen {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: var(--dark);
  color: var(--light);
  text-align: center;
}

h1 {
  font-size: 60px;
  letter-spacing: 1px;
  margin: 0px;
  text-transform: uppercase;
}

h4 {
  font-size: 16px;
  margin: 0px 0px 30px 0px;
}
.mode-box {
  background-color: transparent;
  border: 1px solid var(--light);
  color: var(--light);
  font-size: 18px;
  padding: 14px 25px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
  border-radius: 10px;
  width: 110px;
  height: 110px;
  margin: 0 10px;
}

.mode-box:hover {
  background-color: var(--light);
  color: var(--dark);
}

.mode-box .mode {
  font-size: 25px;
  font-weight: bold;
  margin-bottom: 3px;
  display: block;
}
.mode-box .name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 3px;
  display: block;
}
</style>
