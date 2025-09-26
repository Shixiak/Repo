# transition 组件

## 简介

在 Vue 中，`<transition>` 是一个内置的过渡/动画全局组件。

## 基本用法

```html
<template>
  <button @click="show = !show">切换</button>
  <transition name="fade">
    <p v-if="show">Hello Vue!</p>
  </transition>
</template>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
```

当 `v-if/v-show` 控制的元素插入或移除时，Vue 会自动为它添加以下类名（基于 `name` 属性生成）：

| 阶段     | 进入动画类名               | 离开动画类名               |
| ------ | -------------------- | -------------------- |
| 起始状态   | `.fade-enter-from`   | `.fade-leave-from`   |
| 激活过渡状态 | `.fade-enter-active` | `.fade-leave-active` |
| 结束状态   | `.fade-enter-to`     | `.fade-leave-to`     |

## 类名的具体添加时机

从 渲染流程 的角度分析一下 `<transition>` 中 初始状态 (`*-from`) 是什么时候被应用的。

当一个元素要插入 DOM 并执行过渡时，Vue 内部大致做了以下几步：

1. 渲染 VNode → 挂载真实 DOM
   * 元素还没有被插入页面时，Vue 会先创建对应的真实 DOM 节点。
   * 在这一步，Vue 会给元素加上 `v-enter-from` 和 `v-enter-active` 这两个 class。
   * 所以元素一出生，样式就处于 初始状态（比如透明度 0）。
2. 强制触发一次浏览器重绘 (reflow)
   * Vue 在下一帧（`requestAnimationFrame`）里，强制让浏览器读取元素的某些属性，从而触发一次渲染。
   * 这样浏览器能“看到”元素处在 `*-from` 定义的状态。
3. 切换到目标状态
   * 在上一步重绘之后，Vue 立刻移除 `v-enter-from`，并添加 `v-enter-to`。
   * 此时元素的样式就从 `from` 状态 → `to` 状态，
   * 而由于 `v-enter-active` 上定义了 `transition`，浏览器会用补间动画完成过渡。
4. 过渡结束，清理 class
   * 当 `transitionend` 或 `animationend` 事件触发后，Vue 移除 `v-enter-active` 和 `v-enter-to`，元素样式保持在最终状态。

## 特性

在 Vue 的过渡流程里，`*-enter-to` 是指过渡结束时要应用的样式。如果你没有显式写 `fade-enter-to { opacity: 1 }`，Vue 会让元素回到它原本的最终样式。

## 生命周期钩子

### 钩子列表

Transition 有一些生命周期钩子，方便我们更加灵活的控制过渡动画。

beforeEnter(el)

* 元素插入 DOM 之前调用。
* 这里适合做一些初始状态设置，比如设置透明度为 0。

enter(el, done)

* 元素插入后立即调用。
* 你可以在这里添加动画效果（CSS 类或 JS 动画）。
* 如果使用 JS 动画，需要手动调用 `done()` 表示结束。

afterEnter(el)

* 进入动画完成后调用。
* 常用于清理操作，比如移除过渡类。

enterCancelled(el)

* 如果进入动画被打断（例如元素被快速移除），会触发这个钩子。

beforeLeave(el)

* 离开动画开始前调用。
* 在这里可以设置起始状态。

leave(el, done)

* 离开动画执行时调用。
* 需要在这里触发动画逻辑，完成后调用 `done()`。

afterLeave(el)

* 动画结束，元素被移除后调用。
* 常用于清理。

leaveCancelled(el)

* 如果离开动画被打断（比如元素被重新显示），会调用此钩子。

### 使用方法

这些钩子有两种绑定方式，以 onEnter 为例：

方式 1：通过 v-on 绑定：

```html
<transition @enter="func"></transition>
```

方式 2：通过 v-bind 绑定：

```html
<transition :onEnter="func"></transition>
```

钩子函数由 Vue 内部在过渡阶段调用，参数 `el` 是发生过渡的 DOM 元素。`done` 参数是 Vue 提供的回调，用于手动标记动画结束（仅在 onEnter/onLeave 钩子传入）；

看一个例子吧，一个 collapse。

```html
<template>
  <transition v-bind="hooks">
    <div v-show="show" class="box">
      <p>
        Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.Lorem, ipsum dolor sit amet consectetur adipisicing elit. Non ipsam, provident sunt sit eos quisquam? Debitis fuga voluptatibus velit dolorum ullam, nemo facilis ipsa. Ab pariatur ipsum excepturi amet totam.
      </p>
    </div>
  </transition>

  <button @click="show = !show">切换</button>
</template>

<script setup lang="ts">
import { ref, type TransitionProps } from 'vue'

const show = ref(true)

const hooks: TransitionProps = {
  onBeforeEnter(el) {
    const node = el as HTMLElement
    node.style.height = '0px'
    node.style.overflow = 'hidden'
  },
  onEnter(el) {
    const node = el as HTMLElement
    node.style.transition = 'height 1s ease'
    node.style.height = node.scrollHeight + 'px'
  },
  onAfterEnter(el) {
    const node = el as HTMLElement
    node.style.height = ''
    node.style.overflow = ''
  },
  onBeforeLeave(el) {
    const node = el as HTMLElement
    node.style.height = node.scrollHeight + 'px'
    node.style.overflow = 'hidden'
  },
  onLeave(el) {
    const node = el as HTMLElement
    node.style.transition = 'height 1s ease'
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
</style>
```
