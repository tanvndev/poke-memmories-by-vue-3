<template>
  <div class="card" :class="{ disabled: isDisabled }" :style="cardStyle">
    <div class="card-inner" :class="{ 'is-flipped': isFlipped }">
      <div class="card-face card-face-front" @click="onToggleFlipCard">
        <div class="card-content" :style="{ backgroundSize: cardStyle.backgroundBackSize }"></div>
      </div>
      <div class="card-face card-face-back" @click="onToggleFlipCard">
        <div
          class="card-content"
          :style="{
            backgroundImage: `url(src/assets/images/${props.imgBackFace}.png)`,
            backgroundSize: cardStyle.backgroundSize,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits, defineExpose, computed } from 'vue'
const props = defineProps({
  imgBackFace: {
    type: Number,
    required: true,
  },
  card: {
    type: [Number, String, Array, Object],
    required: true,
  },
  cardContext: {
    type: Array,
    default: () => [],
  },
})
const emit = defineEmits(['onFlip'])

const isFlipped = ref(false)
const isDisabled = ref(false)

const onToggleFlipCard = () => {
  if (isDisabled.value) return false
  isFlipped.value = !isFlipped.value
  if (isFlipped.value) {
    emit('onFlip', props.card)
  }
}

const onFlipBackCard = () => {
  isFlipped.value = false
}

const onDisableCard = () => {
  isDisabled.value = true
}

const cardStyle = computed(() => {
  const cardCount = props.cardContext.length
  const baseSize = (1080 - 16 * 4) / Math.sqrt(cardCount) - 16
  const height = `${baseSize}px`
  const width = `${(baseSize * 3) / 4}px`
  const backgroundSize = `${(baseSize * 3) / 6}px`
  const backgroundBackSize = `${(baseSize * 3) / 10}px`

  return {
    height,
    width,
    backgroundSize,
    backgroundBackSize,
  }
})

// Expose the method to be callable from the parent component
defineExpose({ onFlipBackCard, onDisableCard })
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  width: 100px;
  height: 140px;
  border-radius: 10px;
  margin: 2px 4px;
}
.card.disabled {
  pointer-events: none;
}

.card-inner {
  width: 100%;
  height: 100%;
  transition: transform 0.8s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
  perspective: 1000px;
}

.card-inner.is-flipped {
  transform: rotateY(-180deg);
}
.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 10px;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card-face-front .card-content {
  background: url('../assets/images/icon_back.png') no-repeat center center;
  background-size: 40px;
  height: 100%;
  width: 100%;
}
.card-face-back .card-content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}

.card-face-back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}
</style>
