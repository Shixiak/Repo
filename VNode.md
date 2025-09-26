# VNode

## 简介

在 Vue（无论是 Vue 2 还是 Vue 3）里，**VNode（Virtual Node，虚拟节点）** 对真实 DOM 节点或组件的一种抽象描述。它不是直接操作 DOM，而是用一个普通的 JavaScript 对象 来表达节点的结构和属性，最终由 Vue 的渲染器（renderer）把 VNode 转换成真实的 DOM。
