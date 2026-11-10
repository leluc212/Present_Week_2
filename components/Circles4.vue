<script setup lang="ts">
withDefaults(defineProps<{ color?: string, size?: number }>(), {
  color: 'var(--lpc-navy)',
  size: 12,
})
</script>

<template>
  <div class="flex gap-2 pointer-events-none" aria-hidden="true">
    <span
      v-for="i in 4"
      :key="i"
      class="block rounded-full"
      :style="{ width: `${size}px`, height: `${size}px`, border: `2.5px solid ${color}` }"
    />
  </div>
</template>
