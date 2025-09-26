# Fragment

## 简介

在 Vue 3 的 TSX/JSX 写法里，你可能会看到：

```tsx
import { Fragment } from 'vue'

return () => (
  <Fragment>
    <li>One</li>
    <li>Two</li>
  </Fragment>
)
```

这里的 `Fragment` 是 Vue 提供的一个内置组件标识。它的作用和 React 里的 `<>...</>` 很像：允许你在不添加额外 DOM 节点的情况下，返回多个子节点。

---

## 作用

在 Vue 的渲染函数（包括 TSX）里，一个函数只能返回一个根节点。

比如这样会报错：

```tsx
return () => (
  <div>One</div>
  <div>Two</div>
)
```

因为返回了两个根节点。

解决方法就是用 `Fragment` 包裹：

```tsx
return () => (
  <Fragment>
    <div>One</div>
    <div>Two</div>
  </Fragment>
)
```

编译后，Vue 会把它转成 **一个 children 数组**，而不会生成真正的 `<fragment>` DOM 元素。编译后上面那段代码会变成：

```js
h(Fragment, null, [
  h('div', null, 'One'),
  h('div', null, 'Two')
])
```

> 注意：这里的 `Fragment` 在运行时其实是个 **特殊的标识符**，Vue 内部会识别它，不会渲染出 `<fragment>` 标签，而是直接把子节点渲染出来。

最终 DOM 渲染结果就是：

```html
<div>One</div>
<div>Two</div>
```

没有多余的包裹元素。
