# Popper JS

一个用于实现浮窗的 npm 包。Popper.js 不会自己“生成”DOM 节点，也不会替你挂载到页面上——它只负责计算与应用定位样式。

## createPopper

创建一个 `PopperJS` 实例对象，这个对象负责订阅 DOM 变化，并在需要时重新计算位置。

这一步必须等**组件挂载完成之后**再进行。

`createPopper` 需要真实的 DOM 节点，`@popperjs/core` 的 `createPopper` 签名是：

```ts
createPopper(reference: Element | VirtualElement, popper: HTMLElement, options?: Partial<Options>)
```
  
第一个参数（`reference`）和第二个参数（`popper`）必须是真实的 DOM 元素对象。在 Vue 组件**挂载之前**，`ref()` 获取到的只是一个 `null`（还没渲染），此时传给 `createPopper` 会直接报错或行为异常。

## 简单用例

```html
<template>
  <img src="./assets/vue.svg" alt="vue logo" width="125px" height="125px" ref="triggerNode">
  <div ref="overlayNode">Hello Tooltip</div>
</template>

<script setup lang="ts">
import { createPopper } from '@popperjs/core';
import type { Instance } from '@popperjs/core';
let popperInstance: Instance | null = null;
const overlayNode = ref<HTMLElement>();
const triggerNode = ref<HTMLElement>();

onMounted(() => {
  if(overlayNode.value && triggerNode.value) {
    popperInstance = createPopper(triggerNode.value, overlayNode.value, {
      placement: 'right'
    });
  }
})
</script>
```

## destroy

销毁的方法。
