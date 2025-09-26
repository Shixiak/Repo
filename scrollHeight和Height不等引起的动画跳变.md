# 动画跳变

## 问题描述

示例代码是一段 vue3-ts 代码，引用了 reset.css 文件。

App.vue 的代码如下：

```html
<template>
  <button @click="show = !show">切换</button>
  <transition v-bind="hooks">
    <div class="box" v-show="show">
      <p>
        Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.
      </p>
  </div>
  </transition>
</template>

<script setup lang="ts">
import { ref, type TransitionProps } from 'vue'

const show = ref(false)

const hooks: TransitionProps = {
  onBeforeEnter(el) {
    const node = el as HTMLElement
    node.style.height = '0px'
    node.style.overflow = 'hidden'
  },
  onEnter(el) {
    const node = el as HTMLElement
    node.style.transition = 'height 1.5s ease'
    node.style.height = node.scrollHeight + 'px'
    console.log('scroll height', node.scrollHeight + 'px');
  },
  onAfterEnter(el) {
    const node = el as HTMLElement;
    node.style.height = '';
    node.style.overflow = '';
    console.log('style height', getComputedStyle(node).height);
  },
  onBeforeLeave(el) {
    const node = el as HTMLElement
    node.style.height = node.scrollHeight + 'px'
    node.style.overflow = 'hidden'
  },
  onLeave(el) {
    const node = el as HTMLElement
    node.style.transition = 'height 1.5s ease'
    node.style.height = '0px'
  },
  onAfterLeave(el) {
    const node = el as HTMLElement
    node.style.height = ''
    node.style.overflow = ''
  }
}
</script>

<style>
.box {
  background: lightblue;
}
.wrapper {
  background-color: red;
}
</style>
```

使用 `scrollHeight` 获取到的值为 100px，但是使用 `getComputedStyle(node).height` 获取到的值为 96px，这 4px 的误差会导致滚动范围会略微超过下边界然后跳回下边界。

## 原因

文本排版用到这些字体度量：

* ascent：上伸到基线之上的高度；
* descent：下伸到基线之下的高度（下行/下缘勾）；
* line-gap（leading）：字体推荐的行间隙（有的字体为 0）。

`line-height` 把这些量“打包”成每行的行框高度。自然高度是按行高累计：

$$
H_{\text{used}} = N \times \text{line-height}
$$

但**布局溢出矩形**会在最后一行的下缘，把 `descent/line-gap` 的“尾部空隙”也纳入，且取整到上一个 CSS px；因此：

$$
H_{\text{scroll}} \approx H_{\text{used}} \;+\; \Delta_{\text{descender/gap}} \;+\; \Delta_{\text{ceil}}
$$

其中 $\Delta_{\text{descender/gap}} \sim 1\text{–}3\text{px}$，$\Delta_{\text{ceil}} \in [0,1)\text{px}$，合起来你就看到了 **96 → 100**。

## 参考资料

### 网络资料

[网页字体度量及渲染](https://juejin.cn/post/7242145254056362039)
