# 插槽

## 简介

在 Vue 中，插槽（slot） 是一种让父组件可以向子组件传递内容的机制，本质上其实是一个渲染式函数。它允许父组件在使用子组件时，把一部分结构传递进去，由子组件在指定位置渲染，从而实现灵活的内容分发。

## 插槽渲染的本质

在 Vue 内部，插槽其实就是子组件在渲染时调用父组件传过来的“渲染函数”。

当你写：

```html
<Child>
  <p>hello</p>
</Child>
```

父组件其实把 `<p>hello</p>` 转换成了一个函数，传给了子组件：

```js
slots: {
  default: () => [ h('p', 'hello') ]
}
```

子组件里写的 `<slot/>`，本质就是调用：

```js
render() {
  return this.$slots.default?.()
}
```

所以，插槽就是一个函数，调用它会返回一组 VNode。

## 具名插槽

具名插槽只是把 `default` 换成了不同的 key，比如：

```html
<Child>
  <template #header><h1>标题</h1></template>
  <template #footer><p>页脚</p></template>
</Child>
```

会变成：

```js
slots: {
  header: () => [ h('h1', '标题') ],
  footer: () => [ h('p', '页脚') ]
}
```

子组件里调用：

```js
this.$slots.header?.()
this.$slots.footer?.()
```

---

## 作用域插槽

作用域插槽就是 一个带参数的函数。
子组件把数据传进去，父组件决定怎么渲染：

```html
<!-- 父组件 -->
<List v-slot="{ item }">
  <span>{{ item.name }}</span>
</List>
```

编译后大致等价于：

```js
slots: {
  default: (scope) => [ h('span', scope.item.name) ]
}
```

子组件调用：

```js
this.$slots.default?.({ item })
```

这样数据就能“流”到父组件插槽函数里面。

## 典型案例

子组件

```vue
<!-- ListItem.vue -->
<template>
  <li v-for="(item, idx) in items" :key="idx">
    <slot :item="item"/>
  </li>
</template>
```

这里的 `<slot :item="item"/>`，本质上等价于在 **渲染函数** 里调用：

```js
this.$slots.default?.({ item })
```

也就是说：

* `this.$slots.default` 是一个函数。
* 子组件在遍历 `items` 时，会 **多次调用这个函数**，并且把 `{ item }` 传进去。
* 父组件提供的插槽内容，就会根据这个参数渲染。

父组件使用时

```vue
<ListItem :items="users">
  <template #default="{ item }">
    <span>{{ item.name }}</span>
  </template>
</ListItem>
```

编译后大致等价于：

```js
slots: {
  default: ({ item }) => [ h('span', item.name) ]
}
```

运行时过程

子组件执行 `v-for`，比如有 3 个 `items`：`["A", "B", "C"]`。

每次循环时调用 `this.$slots.default({ item })`：

1. 传 `{ item: "A" }` → 返回 VNode `<span>A</span>`
2. 传 `{ item: "B" }` → 返回 VNode `<span>B</span>`
3. 传 `{ item: "C" }` → 返回 VNode `<span>C</span>`

最终渲染：

```html
<li><span>A</span></li>
<li><span>B</span></li>
<li><span>C</span></li>
```
