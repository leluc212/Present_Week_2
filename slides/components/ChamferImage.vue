<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(defineProps<{
  src?: string
  border?: string
  borderWidth?: number
  cut?: number
  corners?: string
  position?: string
}>(), {
  border: '#ffffff',
  borderWidth: 8,
  cut: 70,
  corners: 'tr bl',
  position: 'center',
})

function poly(k: string) {
  const has = (c: string) => props.corners.split(' ').includes(c)
  return `polygon(${[
    has('tl') ? `0 ${k}, ${k} 0` : '0 0',
    has('tr') ? `calc(100% - ${k}) 0, 100% ${k}` : '100% 0',
    has('br') ? `100% calc(100% - ${k}), calc(100% - ${k}) 100%` : '100% 100%',
    has('bl') ? `${k} 100%, 0 calc(100% - ${k})` : '0 100%',
  ].join(', ')})`
}

const outer = computed(() => poly(`${props.cut}px`))
const inner = computed(() => poly(`${Math.max(props.cut - props.borderWidth * 0.6, 0)}px`))

// Khi chưa có ảnh: nền gradient tối, tông xanh công nghệ
const bg = computed(() => props.src
  ? `url(${props.src}) ${props.position} / cover no-repeat`
  : 'radial-gradient(circle at 30% 30%, #1d6fd8 0%, #0a2a5c 45%, #050d1f 100%)')
</script>

<template>
  <div :style="{ background: border, clipPath: outer, padding: `${borderWidth}px` }">
    <div class="w-full h-full" :style="{ background: bg, clipPath: inner }" />
  </div>
</template>
