<template>
  <div class="card-container">
    <div class="inner" :style="innerStyle">
      <PokeCard
        v-for="(card, index) in props.cardContext"
        :key="index"
        :imgBackFace="card"
        :card="{ index, value: card }"
        :cardContext="props.cardContext"
        ref="cardRefs"
        @onFlip="handleRules"
      />
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref, computed } from 'vue'
import PokeCard from './PokeCard.vue'

const props = defineProps({
  cardContext: {
    type: Array,
    default: () => [],
  },
})
const emit = defineEmits(['onFinish'])

const rules = ref([])
const cardRefs = ref([])

const handleMatch = () => {
  const [firstCard, secondCard] = rules.value
  cardRefs.value[firstCard.index].onDisableCard()
  cardRefs.value[secondCard.index].onDisableCard()

  const disabledElement = document.querySelectorAll('.card.disabled')

  if (disabledElement && props.cardContext.length - 2 === disabledElement.length) {
    setTimeout(() => {
      emit('onFinish')
    }, 1000)
  }
  rules.value = []
}

const handleMismatch = () => {
  setTimeout(() => {
    const [firstCard, secondCard] = rules.value
    cardRefs.value[firstCard.index].onFlipBackCard()
    cardRefs.value[secondCard.index].onFlipBackCard()
    rules.value = []
  }, 800)
}

const handleRules = (card) => {
  if (rules.value.length === 2) return false
  rules.value.push(card)

  if (rules.value.length === 2) {
    if (rules.value[0].value === rules.value[1].value) {
      handleMatch()
    } else {
      handleMismatch()
    }
  }
}

const innerStyle = computed(() => {
  const cardCount = props.cardContext.length
  const cardSize = (((1080 - 16 * 4) / Math.sqrt(cardCount) - 16) * 3) / 4 + 14
  return {
    width: `${cardSize * Math.sqrt(cardCount)}px`,
  }
})
</script>

<style lang="css" scoped>
.card-container {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
  padding: 50px 0;
}

.inner {
  width: 500px;
  margin: 0 auto;
}
</style>
