<template>
  <!-- Slide Q&A: chỉ có chữ lớn ở giữa, trang trí theo mẫu -->
  <div class="slidev-layout lpc-qa">
    <ChamferShape class="absolute" style="left:-40px;top:-30px;width:420px;height:300px" color="var(--lpc-soft)" corners="br" :cut="150" />
    <ChamferShape class="absolute" style="right:0;bottom:0;width:460px;height:330px" color="var(--lpc-navy)" corners="tl" :cut="140" />
    <ChamferShape class="absolute" style="right:380px;bottom:0;width:170px;height:110px" color="var(--lpc-orange)" corners="tl" :cut="55" />
    <ChamferShape class="absolute" style="left:80px;top:0;width:260px;height:70px" color="var(--lpc-orange)" corners="br" :cut="45" />
    <DotGrid class="absolute" style="left:60px;bottom:50px" :rows="7" :cols="7" :gap="12" shape="triangle" />
    <DotGrid class="absolute" style="right:60px;top:60px" :rows="6" :cols="6" :gap="12" shape="triangle" />
    <Circles4 class="absolute" style="left:560px;top:48px" />
    <Circles4 class="absolute" style="right:90px;bottom:40px" color="#fff" />

    <div class="absolute inset-0 flex flex-col items-center justify-center">
      <div class="qa">
        <slot />
      </div>
      <div style="width:220px;height:6px;background:var(--lpc-orange);margin-top:8px" />
    </div>
  </div>
</template>

<style scoped>
.lpc-qa {
  padding: 0;
  position: relative;
  overflow: hidden;
  background: #fff;
}
.qa :deep(h1) {
  font-size: 11rem;
  line-height: 1;
  letter-spacing: 0.04em;
  color: var(--lpc-navy);
  margin: 0;
}
</style>
