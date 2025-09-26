# stubs

## 基本概念

`subs: ['FontAwesomeIcon']` 做了什么？

告诉 VTU：遇到 `<FontAwesomeIcon>` 组件时，用一个 stub（占位组件）替换掉它。该组件会被算染成 `<font-awesome-icon-stub>` 标签，不会执行真正的渲染逻辑。
