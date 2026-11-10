<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  color?: string
  /** độ dài cạnh vát (px) */
  cut?: number
  /** các góc bị vát: tl, tr, br, bl */
  corners?: string
  radius?: number
}>(), {
  color: 'var(--lpc-orange)',
  cut: 60,
  corners: 'tr bl',
  radius: 0,
})

const clip = computed(() => {
  const k = `${props.cut}px`
  const has = (c: string) => props.corners.split(' ').includes(c)
  const pts = [
    has('tl') ? `0 ${k}, ${k} 0` : '0 0',
    has('tr') ? `calc(100% - ${k}) 0, 100% ${k}` : '100% 0',
    has('br') ? `100% calc(100% - ${k}), calc(100% - ${k}) 100%` : '100% 100%',
    has('bl') ? `${k} 100%, 0 calc(100% - ${k})` : '0 100%',
  ]
  return `polygon(${pts.join(', ')})`
})
</script>

<template>
  <div
    class="pointer-events-none"
    aria-hidden="true"
    :style="{ background: color, clipPath: clip, borderRadius: `${radius}px` }"
  >
    <slot />
  </div>
</template>
