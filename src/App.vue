<script setup lang="ts">
import { onMounted, ref } from 'vue';
import WOW from 'wow.js';

const test = ref<any>(); // 点击位置 距离视口顶部
const test2 = ref<any>(); // 事件名

const yinzi = 20; // 滚动速度因子
const layoutRef = ref(); // 父容器
const scrollRef = ref(); // 滚动元素
const scrollHeight = ref(); // 滚动元素高度
const clientHeight = ref(); // 父容器高度 / 浏览器高度
const duration =ref(); // 滚动动画时长
const gdbf = ref(); // 初始状态时，需要滚动的距离
const stop = ref(false); // 是否停止滚动
const style = ref(); // 滚动元素样式
const isMobile = ref<boolean>(); // 是否是移动端

const isMobileFunc = () => {
  return /phone|pad|pod|ios|Android|Mobile|webOS|WebOS|iPhone|iPad|iPod|BlackBerry|IEMobile|MQQBrowser|BrowserNG|Symbian|Windows Phone|Opera Mini/i.test(navigator.userAgent);
}

onMounted(() => {
  const wow = new WOW({
    boxClass: 'wow',
    animateClass: 'animated',
    offset: 0,
    mobile: true,
    live: true
  })
  wow.init()

  isMobile.value = isMobileFunc()

  if (isMobile.value) {
    document.addEventListener("touchstart", (e) => {
      test2.value = "touchstart";
      const touch = e.touches[0];
      test.value = touch.clientY;
      let disY = touch.clientY - scrollRef.value.getBoundingClientRect().top;
      document.ontouchmove = (ev) => {
        test2.value = "ontouchmove";
        stop.value = false;
        const touch_move = ev.touches[0];
        let sTop = touch_move.clientY - disY;
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
      document.ontouchend = async () => {
        test2.value = "ontouchend";
        document.ontouchmove = null;
        document.ontouchend = null;
        
        stop.value = !stop.value;
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
      }
    })
  } else {
    document.addEventListener("mousedown", (e) => {
      test2.value = "mousedown";
      test.value = e.clientY;
      let disY = e.clientY - scrollRef.value.getBoundingClientRect().top;
      document.onmousemove = (ev) => {
        test2.value = "onmousemove";
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
      document.onmouseup = async () => {
        test2.value = "onmouseup";
        document.onmousemove = null;
        document.onmouseup = null;

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
      }
    })
  }
})

window.addEventListener("resize", () => {
  test2.value = "resize";
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

// document.body.addEventListener("click", async () => {
//   test2.value = "click";
//   stop.value = !stop.value
//   if (!stop.value) {
//     const top = await scrollRef.value.getBoundingClientRect().top
//     const haveScrollTop = gdbf.value - top
//     duration.value = -(haveScrollTop * yinzi)
//     if (gdbf.value >= 0) {
//       style.value = {
//         transitionProperty: 'all',
//         transitionTimingFunction: 'linear',
//         transitionDuration: `0ms`,
//         transform: `translate(0px, 0px) translateZ(0px)`
//       }
//     } else {
//       style.value = {
//         transitionProperty: 'all',
//         transitionTimingFunction: 'linear',
//         transitionDuration: `${duration.value}ms`,
//         transform: `translate(0px, ${gdbf.value}px) translateZ(0px)`
//       }
//     }
//   } else {
//     const top = await scrollRef.value.getBoundingClientRect().top
//     duration.value = 0
//     style.value = {
//       transitionProperty: 'all',
//       transitionTimingFunction: 'linear',
//       transitionDuration: `${duration.value}ms`,
//       transform: `translate(0px, ${top}px) translateZ(0px)`
//     }
//   }
// })

const imgLoad = () => {
  console.log(document.querySelector(".scroll img")?.clientHeight);
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
}

const toMap = () => {
  window.open("https://surl.amap.com/9INP5q1k8S6");
}
</script>

<template>
  <div v-show="false" class="test" style="position: fixed; top: 0; right: 0; z-index: 9999;">》{{ isMobile }} | {{ test2 }} | {{ test }} | {{ stop }}《</div>
  <div ref="layoutRef" class="layout">
    <div
      ref="scrollRef"
      class="scroll"
      :style="style"
    >
      <img style="width: 100%; max-width: 666px; height: 100vh; position: fixed; top: 0; left: 0; z-index: 9999;" src="./assets/FitgbYFETg_9B9A2tPhb1jmLCd1F.gif" alt="">
      <img @load="imgLoad" src="./assets/Frame 1-1.png" alt="">
      <img @load="imgLoad" src="./assets/Map.png" @click="toMap" alt="">
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
  width: 0;
  height: 0;
}
.scroll {
  overflow-y: scroll;
}
.scroll img {
  width: 100%;
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
