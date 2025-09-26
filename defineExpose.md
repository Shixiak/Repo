# defineExpose

## 概述

宏函数，只能在 `<script setup>` 的顶层使用。

## 使用案例

子组件：

```vue
<!-- Child.vue -->
<script setup>
function focusInput() {
  console.log("Child input focused");
}

const count = 42;

defineExpose({
  focusInput,
  count
});
</script>
```

父组件：

```vue
<!-- Parent.vue -->
<template>
  <Child ref="childRef" />
  <button @click="childRef.focusInput()">调用子组件方法</button>
  <p>{{ childRef.count }}</p>
</template>

<script setup>
import { ref } from 'vue';
import Child from './Child.vue';

const childRef = ref();
</script>
```
