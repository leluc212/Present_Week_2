<script setup lang="ts">
// (e) Khối xanh bên trái có nhãn pill + thẻ trắng, tiêu đề + nội dung bên phải
defineProps<{ label?: string }>()
</script>

<template>
  <div class="slidev-layout lpc-sp">
    <div class="absolute" style="left:0;top:0;width:445px;height:720px;background:var(--lpc-navy)" />
    <div class="absolute" style="left:452px;top:0;width:7px;height:720px;background:var(--lpc-orange)" />
    <ChamferShape class="absolute" style="left:480px;top:60px;width:110px;height:150px" color="var(--lpc-orange)" corners="tr bl" :cut="40" />
    <ChamferShape class="absolute" style="left:470px;top:500px;width:120px;height:120px" color="var(--lpc-orange)" corners="tl br" :cut="40" />
    <ChamferShape class="absolute" style="right:80px;top:0;width:460px;height:110px" color="var(--lpc-soft)" corners="bl" :cut="80" />
    <ChamferShape class="absolute" style="right:0;bottom:0;width:130px;height:34px" color="var(--lpc-orange)" corners="tl" :cut="26" />
    <DotGrid class="absolute" style="left:55px;bottom:60px" :rows="7" :cols="7" :gap="12" color="#fff" shape="triangle" />
    <Circles4 class="absolute" style="left:790px;top:42px" />

    <div class="absolute left">
      <div v-if="label" class="lpc-pill lavender label">{{ label }}</div>
      <div class="lpc-card left-card">
        <slot name="left" />
      </div>
    </div>

    <div class="absolute right">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.lpc-sp {
  padding: 0;
  position: relative;
  overflow: hidden;
  background: #fff;
}
.left {
  left: 20px;
  top: 120px;
  width: 520px;
}
.label {
  font-size: 1.2rem;
  padding: 0.35rem 1.1rem;
  border-radius: 10px;
  margin-bottom: 1rem;
  margin-left: 10px;
}
.left-card {
  padding: 1.4rem 1.6rem;
}
.right {
  left: 640px;
  right: 60px;
  top: 130px;
  bottom: 70px;
}
.right :deep(h1) {
  text-transform: none;
  font-size: 3rem;
}
</style>
