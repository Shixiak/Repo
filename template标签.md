# template

## 简介

在 Vue 3 里，`<template>` 虽然不渲染真实 DOM，但编译后会生成一个“片段”（`fragment`）`VNode`。

## v-for

key 是给虚拟 DOM diff 用的提示，绑定在 VNode 上即可，不需要也不会出现在真实 DOM 属性里。

key 为什么放在 `<template v-for>`：v-for 所在的节点才是这次循环的“单位”。当你把 v-for 写在 `<template>` 上时，这个“单位”就是该片段（fragment）。给 `<template>` 加 :key，就是给这个片段加 key，Vue 才能正确按 item 的身份进行复用/重排。
