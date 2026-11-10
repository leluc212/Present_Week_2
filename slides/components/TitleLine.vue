<script setup lang="ts">
withDefaults(defineProps<{ width?: string, color?: string }>(), {
  width: '100%',
  color: 'var(--lpc-orange)',
})
</script>

<template>
  <div class="my-4" :style="{ width, height: '3px', background: color }" aria-hidden="true" />
</template>
