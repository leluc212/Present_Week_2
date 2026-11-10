<script setup lang="ts">
// (c) Ảnh hoặc khối xanh bên trái, tiêu đề + nội dung bên phải.
// Nếu không có ảnh, slot "left" được đặt trong khung trắng (dùng cho sơ đồ).
defineProps<{ image?: string }>()
</script>

<template>
  <div class="slidev-layout lpc-tr">
    <ChamferShape class="absolute" style="left:0;top:0;width:410px;height:720px" color="var(--lpc-navy)" corners="br" :cut="230" />
    <div class="absolute" style="left:455px;top:0;width:7px;height:720px;background:var(--lpc-orange)" />
    <ChamferShape class="absolute" style="left:500px;top:145px;width:120px;height:160px" color="var(--lpc-orange)" corners="tr bl" :cut="45" />
    <ChamferShape class="absolute" style="left:480px;top:410px;width:140px;height:130px" color="var(--lpc-orange)" corners="tl br" :cut="45" />
    <ChamferShape class="absolute" style="right:80px;top:0;width:470px;height:120px" color="var(--lpc-soft)" corners="bl" :cut="80" />
    <ChamferShape class="absolute" style="right:0;bottom:0;width:130px;height:34px" color="var(--lpc-orange)" corners="tl" :cut="26" />
    <DotGrid class="absolute" style="left:55px;top:515px" :rows="7" :cols="7" :gap="12" color="#fff" shape="triangle" />
    <Circles4 class="absolute" style="left:790px;top:42px" />

    <ChamferImage
      v-if="image"
      class="absolute"
      style="left:115px;top:75px;width:470px;height:575px"
      :src="image"
      corners="tr bl"
      :cut="110"
      :border-width="9"
    />
    <div v-else class="absolute left-box lpc-card">
      <slot name="left" />
    </div>

    <div class="absolute right">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.lpc-tr {
  padding: 0;
  position: relative;
  overflow: hidden;
  background: #fff;
}
.left-box {
  left: 70px;
  top: 90px;
  width: 520px;
  height: 540px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.right {
  left: 680px;
  right: 60px;
  top: 130px;
  bottom: 70px;
}
.right :deep(h1) {
  text-transform: none;
  font-size: 3rem;
}
</style>
