# ref

## 简介

ref 用来定义响应式变量.

## 捕获组件或 DOM 节点

```html
<script>
const buttonRef = ref();
</script>

<template>
  <button ref="buttonRef">Button</button>
</template>
```

当我们在 `<script>` 里面定义了一个 ref 变量, 并在模板的某个组件的 ref 属性上填入这个变量名, 就能使得该变量指向这个组件或者 DOM 节点.
