<script setup lang="ts">
defineProps<{ image?: string, image2?: string }>()
</script>

<template>
  <!-- (f) Ảnh bên trái, pill xanh bên phải -->
  <div class="slidev-layout lpc-ty">
    <ChamferShape class="absolute" style="left:580px;top:0;width:330px;height:150px" color="var(--lpc-soft)" corners="br" :cut="90" />
    <ChamferShape class="absolute" style="right:0;top:350px;width:170px;height:310px" color="var(--lpc-soft)" corners="tl" :cut="80" />
    <ChamferShape class="absolute" style="left:0;top:120px;width:530px;height:560px" color="var(--lpc-navy)" corners="tl br" :cut="130" />
    <ChamferShape class="absolute" style="left:400px;bottom:0;width:120px;height:40px" color="var(--lpc-navy)" corners="tr" :cut="30" />
    <ChamferShape class="absolute" style="right:110px;bottom:0;width:110px;height:40px" color="var(--lpc-orange)" corners="tr" :cut="30" />
    <div class="absolute" style="left:420px;top:390px;width:180px;height:230px;border:7px solid var(--lpc-orange);border-left:none;border-radius:0 20px 20px 0" />
    <ChamferImage class="absolute" style="left:85px;top:75px;width:490px;height:410px" :src="image" corners="tr bl" :cut="90" :border-width="9" />
    <ChamferImage class="absolute" style="left:-20px;top:400px;width:390px;height:255px" :src="image2" corners="tr bl" :cut="60" :border-width="9" />
    <DotGrid class="absolute" style="left:20px;top:25px" :rows="6" :cols="6" :gap="12" />
    <DotGrid class="absolute" style="left:505px;top:545px" :rows="5" :cols="5" :gap="10" />
    <Circles4 class="absolute" style="left:790px;top:42px" />
    <Circles4 class="absolute" style="left:50px;bottom:24px" color="var(--lpc-orange)" />

    <div class="absolute right">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.lpc-ty {
  padding: 0;
  position: relative;
  overflow: hidden;
  background: #fff;
}
.right {
  left: 700px;
  right: 0;
  top: 260px;
}
.right :deep(p) {
  font-size: 1.05rem;
}
</style>
