# h 函数

h，hyperscript 的缩写，是 Vue 中的虚拟节点（VNode）工厂函数，用于创建组件或者 HTML 元素的虚拟节点结构。Vue3 中 它的核心签名如下（简化版，自 @vue/runtime-core）：

```js
function h(
  type: string | Component,
  propsOrChildren?: RawProps | RawChildren | null,
  children?: RawChildren
): VNode
```

## 普通 html 标签

创建一个普通的`<div>` 标签：

```js
h('div', { class: 'box' }, 'Hello world!');
```

渲染结果：

```html
<div class="box">Hello world!</div>
```

## 嵌套 html 标签

```js
h('div', { class: 'parent' }, [
  h('p', '段落1'),
  h('p', '段落2')
])
```

## 组件

组件 `MyComponent.vue` 的定义：

```vue
<script>
  defineProps(['msg']);
</script>

<template>
  <div>{{ msg }}</div>
</template>
```

使用 h 函数渲染：

```js
h(MyComponent, { msg: 'Hello from props!'});
```
