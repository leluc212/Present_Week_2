<template>
  <div class="slidev-layout lpc-agenda">
    <!-- trang trí theo mẫu -->
    <ChamferShape class="absolute" style="left:85px;top:-10px;width:270px;height:90px" color="var(--lpc-orange)" corners="br" :cut="70" />
    <ChamferShape class="absolute" style="left:820px;top:170px;width:470px;height:240px" color="var(--lpc-soft)" corners="tl tr" :cut="110" />
    <ChamferShape class="absolute" style="left:0;top:340px;width:110px;height:380px" color="var(--lpc-navy)" corners="tr" :cut="0" :radius="0" />
    <ChamferShape class="absolute" style="right:55px;bottom:0;width:210px;height:90px" color="var(--lpc-orange)" corners="tl" :cut="70" />
    <div class="absolute" style="left:565px;top:160px;width:720px;height:3px;background:var(--lpc-navy)" />
    <Circles4 class="absolute" style="left:40px;top:118px" color="var(--lpc-orange)" />
    <Circles4 class="absolute" style="right:270px;bottom:40px" />
    <DotGrid class="absolute" style="right:50px;top:55px" :rows="7" :cols="7" :gap="12" shape="triangle" />
    <DotGrid class="absolute" style="left:115px;bottom:15px" :rows="6" :cols="6" :gap="11" color="var(--lpc-orange)" shape="triangle" />

    <div class="absolute title">
      <slot name="title" />
    </div>
    <div class="absolute body">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.lpc-agenda {
  padding: 0;
  position: relative;
  overflow: hidden;
  background: #fff;
}
.title {
  left: 290px;
  top: 82px;
}
.body {
  left: 120px;
  right: 80px;
  top: 195px;
  bottom: 70px;
}
/* các mục thành viên thuốc 2 cột */
.body :deep(.agenda-grid) {
  display: grid;
  grid-template-columns: 1fr 1fr;
  column-gap: 120px;
  row-gap: 52px;
  align-items: start;
}
.body :deep(.agenda-item) {
  justify-self: start;
  background: #b8bec9;
  color: var(--lpc-ink);
  border-radius: 8px;
  padding: 0.4rem 1rem;
  font-size: 1.65rem;
  line-height: 1.4;
}
</style>
