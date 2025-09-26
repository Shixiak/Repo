# Partial

## 简介

`Partial<T>` 是 TypeScript 内置的**工具类型**（Utility Type），它的作用是**将某个类型 `T` 的所有属性变为可选属性**。

## 使用方法

原始类型如下：

```ts
interface User {
  name: string;
  age: number;
  active: boolean;
}
```

使用 `Partial`：

```ts
type UserPartial = Partial<User>;
```

等价于：

```ts
type UserPartial = {
  name?: string;
  age?: number;
  active?: boolean;
};
```
