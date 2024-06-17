<script setup lang="ts">
import { onMounted, ref } from 'vue';

const yinzi = 20; // 滚动速度因子
const layoutRef = ref(); // 父容器
const scrollRef = ref(); // 滚动元素
const scrollHeight = ref(); // 滚动元素高度
const clientHeight = ref(); // 父容器高度 / 浏览器高度
const duration =ref(); // 滚动动画时长
const gdbf = ref() // 初始状态时，需要滚动的距离
const stop = ref(false) // 是否滚动
const style = ref() // 滚动元素样式
onMounted(() => {
  scrollHeight.value = scrollRef.value.scrollHeight;
  clientHeight.value = layoutRef.value.clientHeight;
  gdbf.value = clientHeight.value - scrollHeight.value
  duration.value = -(gdbf.value * yinzi)
  if (gdbf.value >= 0) {
    style.value = {
      transitionProperty: 'all',
      transitionTimingFunction: 'linear',
      transitionDuration: `0ms`,
      transform: `translate(0px, 0px) translateZ(0px)`
    }
  } else {
    style.value = {
      transitionProperty: 'all',
      transitionTimingFunction: 'linear',
      transitionDuration: `${duration.value}ms`,
      transform: `translate(0px, ${gdbf.value}px) translateZ(0px)`
    }
  }
})

window.addEventListener("resize", () => {
  scrollHeight.value = scrollRef.value.scrollHeight;
  clientHeight.value = layoutRef.value.clientHeight;
  gdbf.value = clientHeight.value - scrollHeight.value
  duration.value = -(gdbf.value * yinzi)
  if (gdbf.value >= 0) {
    style.value = {
      transitionProperty: 'all',
      transitionTimingFunction: 'linear',
      transitionDuration: `0ms`,
      transform: `translate(0px, 0px) translateZ(0px)`
    }
  } else {
    style.value = {
      transitionProperty: 'all',
      transitionTimingFunction: 'linear',
      transitionDuration: `${duration.value}ms`,
      transform: `translate(0px, ${gdbf.value}px) translateZ(0px)`
    }
  }
})

document.body.addEventListener("click", async () => {
  stop.value = !stop.value
  if (!stop.value) {
    const top = await scrollRef.value.getBoundingClientRect().top
    const haveScrollTop = gdbf.value - top
    duration.value = -(haveScrollTop * yinzi)
    if (gdbf.value >= 0) {
      style.value = {
        transitionProperty: 'all',
        transitionTimingFunction: 'linear',
        transitionDuration: `0ms`,
        transform: `translate(0px, 0px) translateZ(0px)`
      }
    } else {
      style.value = {
        transitionProperty: 'all',
        transitionTimingFunction: 'linear',
        transitionDuration: `${duration.value}ms`,
        transform: `translate(0px, ${gdbf.value}px) translateZ(0px)`
      }
    }
  } else {
    const top = await scrollRef.value.getBoundingClientRect().top
    duration.value = 0
    style.value = {
      transitionProperty: 'all',
      transitionTimingFunction: 'linear',
      transitionDuration: `${duration.value}ms`,
      transform: `translate(0px, ${top}px) translateZ(0px)`
    }
  }
})

document.addEventListener("mousedown", (e) => {
  let disY = e.clientY - scrollRef.value.getBoundingClientRect().top;
  document.onmousemove = (ev) => {
    stop.value = false
    let sTop = ev.clientY - disY;
    if (sTop < gdbf.value) {
      sTop = gdbf.value
    }
    if (sTop > 0) {
      sTop = 0
    }
    duration.value = 0
    style.value = {
      transitionProperty: 'all',
      transitionTimingFunction: 'linear',
      transitionDuration: `${duration.value}ms`,
      transform: `translate(0px, ${sTop}px) translateZ(0px)`
    }
  };
  document.onmouseup = () => {
    document.onmousemove = null;
    document.onmouseup = null;
  }
})
</script>

<template>
  <div ref="layoutRef" class="layout">
    <div
      ref="scrollRef"
      class="scroll"
      :style="style"
    >
      <div v-for="i in 50" :key="i">
        {{ i }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.layout {
  height: 100vh;
  max-width: 666px;
  margin: 0 auto;
  overflow: hidden;
}
.layout::-webkit-scrollbar {
  display: none;
}
</style>
