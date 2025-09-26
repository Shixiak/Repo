# v-on

## 简介

在 Vue 里，`v-on`（或者简写 `@`）用来监听事件。

## 修饰符

而修饰符是用来改变事件触发方式或行为的特殊语法。

常用的 `v-on` 修饰符：

### 默认行为与冒泡

`.prevent`: 自动调用 `event.preventDefault()`，阻止默认行为。

```vue
<form @submit.prevent="onSubmit">...</form>
```

`.stop`: 自动调用 `event.stopPropagation()`，阻止事件冒泡。

```vue
<button @click.stop="onClick">阻止冒泡</button>
```

组合用法：修饰符可以链式组合：

```vue
<form @submit.prevent.stop="onSubmit">...</form>
```

## 本质

以 `@click` 举例：

`@click="fn"` 是 Vue 的模板语法糖，会在编译阶段转译成 `onClick` 这样的 `VNode props`。Vue 内部渲染 `VNode` 时，会把 `onClick` 识别为 DOM 事件监听，用 `addEventListener` 绑定到元素上。

下面这段模板代码：

```html
<button @click="handleClick">点我</button>
```

最终等价于：

```ts
h('button', { onClick: handleClick }, '点我')
```

## 对象绑定

```html
<template>
  <div v-on="event">
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const isShow = ref(true)
function open() {
  isShow.value = true;
}
function close() {
  isShow.value = false;
}

const events = {
  'mouseenter': open;
  'mouseleave': close;
}
</script>
```
