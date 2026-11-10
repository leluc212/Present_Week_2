<template>
  <!-- (d) Tiêu đề + nội dung bên trái, thẻ trắng trên nền xanh bên phải -->
  <div class="slidev-layout lpc-tlc">
    <ChamferShape class="absolute" style="left:-40px;top:-30px;width:380px;height:390px" color="var(--lpc-soft)" corners="br" :cut="150" />
    <div class="absolute navy" />
    <div class="absolute" style="left:750px;top:640px;width:7px;height:120px;background:var(--lpc-orange);transform:skewX(-35deg)" />
    <ChamferShape class="absolute" style="left:560px;top:0;width:150px;height:60px" color="var(--lpc-orange)" corners="bl" :cut="40" />
    <Circles4 class="absolute" style="left:530px;top:20px" />
    <Circles4 class="absolute" style="right:85px;bottom:30px" color="#fff" />
    <DotGrid class="absolute" style="right:55px;top:55px" :rows="6" :cols="6" :gap="12" shape="triangle" />
    <DotGrid class="absolute" style="left:30px;bottom:20px" :rows="5" :cols="6" :gap="12" shape="triangle" />

    <div class="absolute left">
      <slot />
    </div>
    <div class="absolute card lpc-card">
      <slot name="right" />
    </div>
  </div>
</template>

<style scoped>
.lpc-tlc {
  padding: 0;
  position: relative;
  overflow: hidden;
  background: #fff;
}
.navy {
  left: 690px;
  top: 60px;
  width: 640px;
  height: 720px;
  background: var(--lpc-navy);
  border-radius: 120px 0 0 0;
}
.left {
  left: 60px;
  top: 40px;
  width: 590px;
  bottom: 50px;
}
.card {
  left: 760px;
  top: 150px;
  right: 40px;
  bottom: 90px;
  overflow: hidden;
}
</style>
