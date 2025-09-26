# 全局 render 函数

## 简介

这是 Vue 核心库里导出的全局渲染函数，它的主要作用是：

* 把一个 VNode（虚拟节点树） 渲染到一个真实的 DOM 容器中。
* 它通常配合 `h()`（createVNode）来使用。

示例：

```ts
import { h, render } from 'vue'

const vnode = h('div', { class: 'hello' }, 'Hello World')

// 把 vnode 渲染到 #app 里
render(vnode, document.getElementById('app')!)
```

这里的 `render` 更像是一个“挂载”函数，直接驱动虚拟 DOM -> 真实 DOM 的过程。

在 Vue 里你用到的 `render` 是运行时提供的 **渲染函数**，它的作用是：
把一个虚拟节点（VNode）挂载到一个真实的 DOM 容器中。

结合你的代码来看：

```ts
const vnode = h(MessageConstructor, newProps);  // 1. 生成一个虚拟节点
render(vnode, container);                       // 2. 把 vnode 渲染到 container 里
document.body.appendChild(container.firstElementChild!); // 3. 把 container 的子节点插入到页面
```

## 注意

`render(vnode, container)` 会把传入的 `container` 的内容整个替换掉，变成你渲染的 `vnode` 对应的 DOM 树。如果 `container` 原来有子节点，会被清空掉。之后只保留由 `vnode` 渲染出来的 DOM。
