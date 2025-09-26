# Font Awesome

## 简介

一个图标库。

## 配置

1\. 添加 Vue 组件

```cmd
pnpm i @fontawesome/vue-fontawesome@latest
```

2\. 添加 SVG 核心

```cmd
pnpm i @fontawesome/fontawesome-svg-core
```

3\. 添加图标包

```cmd
pnpm install @fortawesome/free-solid-svg-icons
```

## 在 Vue 中使用图标

在 `main.ts` 中加入以下代码，

```js
import { library } from '@fortawesome/fontawesome-svg-core'
import { fas } from '@fortawesome/free-solid-svg-icons'
library.add(fas);
```

分别讲解以下这几行代码的含义：

`import { library } from '@fortawesome/fontawesome-svg-core';`

这行代码引入了 `@fortawesome/fontawesome-svg-core` 包中的 `library` 对象。`library` 是 FontAwesome 提供的一个功能，它允许你将图标添加到图标库中，方便在应用的任何地方使用。

`import { fas } from '@fortawesome/free-solid-svg-icons';`

这行代码从 `@fortawesome/free-solid-svg-icons` 包中导入了 **固体图标（Solid icons）** 集合中的所有图标。`fas` 是一个包含了 FontAwesome 所有 solid 类型图标的集合。

`library.add(fas);`

这行代码将 `fas` 图标集添加到 `library` 中。通过将图标集添加到 `library`，你就可以在项目中引用和使用这些图标了。这样，FontAwesome 的所有图标都会被加载到库中，随时可以在任何地方使用。

这样就可以在 App.vue 或者其他组件中使用了：

```html
<template>
  <font-awesome-icon icon="arrow-up"></font-awesome-icon>
</template>


<script setup lang="ts">
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
</script>
```
