# PropType

## 简介

PropType 是 Vue 在 类型层面提供的一个工具类型，本质上是个 TypeScript 辅助类型。

## 作用

在 Vue 里，props 的 type 一般要写成运行时的构造函数，比如：

```ts
props: {
  msg: {
    type: String,
    default: ''
  }
}
```

这样写：

运行时 Vue 能检查 msg 必须是字符串

编译时 TS 只会认为 `msg: string`（过于宽泛，没有约束力）

如果我们需要更复杂的类型（比如联合类型、函数、对象），String 这种构造函数就不够用了。

这时，就要靠 `PropType<T>` 来“桥接”运行时和编译时。限制为联合类型

```ts
trigger: {
  type: String as PropType<'hover' | 'click'>,
  default: 'hover'
}
```

TS 类型：'hover' | 'click'

运行时仍然校验它必须是字符串。
