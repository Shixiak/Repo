# TSX

TSX 其实就是 TypeScript + JSX 的缩写。

## JSX

JSX（JavaScript XML）是一种 **在 JavaScript 里写 HTML 标签** 的语法扩展。它最早在 **React** 中流行起来，用来更直观地描述 UI 结构。浏览器本身并不能直接理解 JSX，打包工具（如 Babel、Vite、Webpack）会把它编译成纯 JS 调用。

例如：

```jsx
const element = <div className="box">Hello</div>
```

会被编译成：

```js
const element = React.createElement("div", { className: "box" }, "Hello");
```

createELement 是 React 中提供的创建标签的 JS 函数。

### TSX

* TSX 就是 **带有 TypeScript 类型支持的 JSX**。
* 文件后缀一般是 `.tsx`（而不是 `.jsx`）。
* 作用是：你既能用 JSX 的语法写界面，又能用 TypeScript 的类型系统来做类型检查。

例如在 Vue 3 里：

```tsx
const renderItem = (name: string) => {
  return <li>{name}</li>
}
```

TypeScript 会保证 `name` 一定是字符串，不然就报错。

## 插槽传递

在 **Vue 3 + TSX** 里，传递插槽有两种等价写法：

### children 传“插槽对象”

```tsx
<Tooltip>
  {{
    default: () => slots.default?.(),
    content: () => (
      <ul class="se-dropdown__menu">
        { options.value }
      </ul>
    )
  }}
</Tooltip>
```

这种方法的特点：

* 把一个 对象 当作组件的唯一子节点传入；
* 这个对象的 key 是插槽名（`default`、`content`），value 是返回 VNode 的函数；
* Vue 会把它识别为“插槽集合”。
* 这就是 children-as-slots 模式。

## v-slots 属性写法

```tsx
<Tooltip
  v-slots={{
    default: () => slots.default?.(),
    content: () => (
      <ul class="se-dropdown__menu">{ options.value }</ul>
    )
  }}
/>
```

本质上和上面那种是同一个东西，只是写在属性上；两种写法都被官方 JSX 插件支持，任选其一即可。
