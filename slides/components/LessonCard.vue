<script setup lang="ts">
withDefaults(defineProps<{ title?: string }>(), { title: 'Bài học' })
</script>

<template>
  <div class="lesson">
    <div class="lesson-head">
      <span class="lesson-icon">!</span>
      <span>{{ title }}</span>
    </div>
    <div class="lesson-body">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.lesson {
  border-left: 5px solid var(--lpc-orange);
  background: #fff6ea;
  border-radius: 0 16px 16px 0;
  padding: 0.7rem 1.2rem;
}
.lesson-head {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 700;
  color: #b35f00;
  font-size: 0.95rem;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}
.lesson-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1.4rem;
  height: 1.4rem;
  border-radius: 999px;
  background: var(--lpc-orange);
  color: #fff;
  font-size: 0.9rem;
}
.lesson-body {
  margin-top: 0.2rem;
  font-size: 1.25rem;
  line-height: 1.5;
}
.lesson-body :deep(p) {
  margin: 0;
  font-size: 1.25rem;
}
</style>
