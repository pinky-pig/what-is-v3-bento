<script setup lang="ts">
import { ref } from 'vue'
import { useResizeObserver } from '@vueuse/core'
import V3bento from '../../packages/v3-bento/src'
// import V3bento from '../../packages/v3-bento/dist/v3-bento.es.js'
const print = (val: string, e: any) => {
  // eslint-disable-next-line no-console
  console.log(val, e)
}

const maximumCells = ref(4)
const size = ref(180)
const gap = ref(10)

const containerRef = ref(null)
useResizeObserver(containerRef, (entries) => {
  const entry = entries[0]
  const { width } = entry.contentRect

  if (Math.ceil(width / (size.value + gap.value)) <= 6) {
    maximumCells.value = Math.ceil(width / (size.value + gap.value))
    size.value = 180
  }
  else {
    maximumCells.value = 6
    size.value = 240
  }
})
</script>

<template>
  <div ref="containerRef" class="container">
    <V3bento
      class="bento-container"
      :size="size"
      :disabled="false"
      :gap="gap"
      :maximum-cells="maximumCells"
      @drag-start="print('drag-start', $event)"
      @drag-end="print('drag-end', $event)"
    />
  </div>
</template>

<style scoped>
.container {
  height: 60vh;
  width: 80vw;
  border: 1px solid black;
  margin-left: auto;
  margin-right: auto;
  display: grid;
  place-items: center;
}

.bento-container {
  border: 1px solid rgb(176, 108, 108);
  margin-left: auto;
  margin-right: auto;
}
</style>
