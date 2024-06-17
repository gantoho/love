<script setup lang="ts">
import { onMounted, ref } from 'vue';
import WOW from 'wow.js'

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
  const wow = new WOW({
    boxClass: 'wow',
    animateClass: 'animated',
    offset: 0,
    mobile: true,
    live: true
  })
  wow.init()

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
      <div class="item item_1">
        <img
          class="wow fadeInUp"
          data-wow-delay="0.1s"
          src="https://files.ganto.cn/files/2.jpg"
          alt=""
        >
        <img
          class="wow fadeInUp"
          data-wow-delay="0.1s"
          src="https://files.ganto.cn/files/129.jpg"
          alt=""
        >
      </div>
      <div class="item item_2">
        <img
          class="wow fadeInUp"
          data-wow-delay="0.1s"
          src="https://files.ganto.cn/files/8.jpg"
          alt=""
        >
        <img
          src="https://files.ganto.cn/files/129.jpg"
          alt=""
        >
        <img
          src="https://files.ganto.cn/files/81.jpg"
          alt=""
        >
      </div>
      <div>
        <p>11</p>
        <p>21</p>
        <p>31</p>
        <p>41</p>
        <p>51</p>
        <p>61</p>
        <p>71</p>
        <p>81</p>
        <p>91</p>
        <p>101</p>
        <p>111</p>
        <p>121</p>
        <p>131</p>
        <p>141</p>
        <p>151</p>
        <p>161</p>
        <p>171</p>
        <p>181</p>
        <p>191</p>
        <p>201</p>
        <p>211</p>
        <p>221</p>
        <p>231</p>
        <p>241</p>
        <p>251</p>
        <p>261</p>
        <p>271</p>
        <p>281</p>
        <p>291</p>
        <p>301</p>
        <p>311</p>
        <p>321</p>
        <p>331</p>
        <p>341</p>
        <p>351</p>
        <p>361</p>
        <p>371</p>
        <p>381</p>
        <p>391</p>
        <p>401</p>
        <p>411</p>
        <p>421</p>
        <p>431</p>
        <p>441</p>
        <p>451</p>
        <p>461</p>
        <p>471</p>
        <p>481</p>
        <p>491</p>
        <p>501</p>
        <p>511</p>
        <p>521</p>
        <p>531</p>
        <p>541</p>
        <p>551</p>
        <p>561</p>
        <p>571</p>
        <p>581</p>
        <p>591</p>
        <p>601</p>
        <p>611</p>
        <p>621</p>
        <p>631</p>
        <p>641</p>
        <p>651</p>
        <p>661</p>
        <p>671</p>
        <p>681</p>
        <p>691</p>
        <p>701</p>
        <p>711</p>
        <p>721</p>
        <p>731</p>
        <p>741</p>
        <p>751</p>
        <p>761</p>
        <p>771</p>
        <p>781</p>
        <p>791</p>
        <p>801</p>
        <p>811</p>
        <p>821</p>
        <p>831</p>
        <p>841</p>
        <p>851</p>
        <p>861</p>
        <p>871</p>
        <p>881</p>
        <p>891</p>
        <p>901</p>
        <p>911</p>
        <p>921</p>
        <p>931</p>
        <p>941</p>
        <p>951</p>
        <p>961</p>
        <p>971</p>
        <p>981</p>
        <p>991</p>
        <p>1001</p>
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
.scroll .item {
  width: 100%;
  overflow: hidden;
}
.scroll .item img {
  width: 100%;
}
.scroll .item_1 {
  height: 900px;
}
</style>
