<script setup lang="ts">
const props = withDefaults(defineProps<{
  rows?: number
  cols?: number
  gap?: number
  size?: number
  color?: string
  /** 'square' = lưới chữ nhật, 'triangle' = lưới tam giác như góc mẫu */
  shape?: 'square' | 'triangle'
}>(), {
  rows: 6,
  cols: 8,
  gap: 14,
  size: 3,
  color: 'var(--lpc-ink)',
  shape: 'square',
})

function visible(r: number, c: number) {
  if (props.shape === 'square')
    return true
  return c <= r
}
</script>

<template>
  <svg
    :width="cols * gap"
    :height="rows * gap"
    class="pointer-events-none"
    aria-hidden="true"
  >
    <template v-for="r in rows" :key="r">
      <template v-for="c in cols" :key="c">
        <circle
          v-if="visible(r, c)"
          :cx="(c - 0.5) * gap"
          :cy="(r - 0.5) * gap"
          :r="size / 2"
          :fill="color"
        />
      </template>
    </template>
  </svg>
</template>
