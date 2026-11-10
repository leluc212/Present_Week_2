<script setup lang="ts">
defineProps<{ image?: string, image2?: string }>()
</script>

<template>
  <div class="slidev-layout lpc-cover">
    <!-- mảng nền nhạt -->
    <ChamferShape class="absolute" style="left:-80px;top:-40px;width:430px;height:300px" color="var(--lpc-soft)" corners="br" :cut="160" />
    <ChamferShape class="absolute" style="left:300px;bottom:0;width:300px;height:120px" color="var(--lpc-soft)" corners="tl" :cut="80" />
    <ChamferShape class="absolute" style="right:30px;top:0;width:260px;height:75px" color="var(--lpc-soft)" corners="bl" :cut="50" />

    <!-- cụm hình bên phải -->
    <ChamferShape class="absolute" style="left:675px;top:40px;width:260px;height:240px" color="var(--lpc-orange)" corners="tl" :cut="70" />
    <ChamferShape class="absolute" style="right:30px;top:95px;width:150px;height:140px" color="var(--lpc-navy)" corners="tr bl" :cut="55" />
    <ChamferShape class="absolute" style="left:870px;top:330px;width:380px;height:355px" color="var(--lpc-navy)" corners="tl" :cut="90" />
    <ChamferShape class="absolute" style="left:680px;top:560px;width:180px;height:120px" color="var(--lpc-orange)" corners="br" :cut="55" />
    <ChamferImage class="absolute" style="left:740px;top:92px;width:480px;height:410px" :src="image" corners="tr bl" :cut="95" :border-width="9" />
    <ChamferImage class="absolute" style="left:635px;top:418px;width:365px;height:190px" :src="image2" border="var(--lpc-orange)" corners="tr bl" :cut="40" :border-width="9" />
    <DotGrid class="absolute" style="left:675px;top:35px" :rows="5" :cols="5" :gap="12" />
    <DotGrid class="absolute" style="right:25px;bottom:20px" :rows="7" :cols="7" :gap="12" shape="triangle" />
    <Circles4 class="absolute" style="right:200px;top:48px" />
    <Circles4 class="absolute" style="left:965px;bottom:42px" color="#fff" />
    <Circles4 class="absolute" style="left:50px;bottom:36px" color="var(--lpc-ink)" />

    <!-- nội dung -->
    <div class="absolute content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.lpc-cover {
  padding: 0;
  position: relative;
  overflow: hidden;
  background: #fff;
}
.content {
  left: 72px;
  top: 150px;
  width: 560px;
}
.content :deep(h1) {
  font-size: 3.9rem;
  letter-spacing: 0.03em;
  line-height: 1.15;
}
.content :deep(p) {
  margin: 0.3rem 0;
  font-size: 1.3rem;
}
</style>
