# setup

## 简介

在 Vue 3 中，`setup` 函数是组合式 API 的核心入口点。setup 相当于组件的初始化逻辑函数。它会在组件实例创建之前执行（在 `beforeCreate` 和 `created` 之间），用来声明响应式状态、计算属性、方法、生命周期钩子等。

## 函数签名

```ts
setup(props, context) {
  // ...
}
```

它接收两个参数：

### props

一个 **响应式只读对象**，包含从父组件传入的所有 props。不能直接修改 `props`，因为它是只读的。如果需要根据 props 派生新的状态，应当使用 `ref` 或 `computed`。

* 示例：

```ts
export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title) // 父组件传入的 title
  }
}
```

### context

`context` 不是响应式的，但包含了三个常用属性：

`attrs`

* 父组件传入但未被声明为 `props` 的 attribute。
* 相当于 Vue 2 里的 `$attrs`。
* 示例：

  ```ts
  setup(props, { attrs }) {
    console.log(attrs.class) // 未声明为 props 的 class、id 等
  }
  ```

`slots`

* 插槽对象，相当于 Vue 2 的 `$slots`。
* 可以用来调用插槽渲染函数。
* 示例：

  ```ts
  setup(props, { slots }) {
    return () => slots.default ? slots.default() : 'No content';
  }
  ```

`emit`

* 触发自定义事件的函数，相当于 Vue 2 的 `$emit`。
* 示例：

  ```ts
  setup(props, { emit }) {
    const clickHandler = () => {
      emit('update', 'newValue');
    }
    return { clickHandler }
  }
  ```

## 返回值

`setup` 的返回值有两种写法：

**返回一个对象**：模板中可以直接使用。

```ts
setup() {
  const count = ref(0);
  const increment = () => count.value++;
  return { count, increment };
}
```

模板中：

```vue
<button @click="increment">{{ count }}</button>
```

**返回一个渲染函数**：直接定义组件的渲染逻辑。

```ts
setup() {
  return () => h('div', 'Hello Vue 3');
}
```
