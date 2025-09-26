# Mocked

## 简介

`Mocked<T>` 是 **Vitest（以及 Jest）提供的 TypeScript 类型工具**，用于把一个已有类型 **T** 的所有属性和方法，变成“mock 版本”的类型。

简单理解：

* 它不是运行时代码（编译后消失）
* 它只是一个**类型声明**，帮助 TypeScript 知道“这个对象已经被 mock 了”
* 转换后的每个方法属性会自动支持 `.mockReturnValue()`、`.mockResolvedValue()`、`.mockImplementation()` 等方法签名

## 为什么要有它

假设我们直接 `vi.mock('axios')`，运行时的 `axios.get` 已经是 `vi.fn()` 了，但**TS 类型信息**还是原来的 `axios.get` 签名：

```ts
import axios from 'axios'
vi.mock('axios')

// /
// 这里 TypeScript 会报错，因为 axios.get 不是 mock 函数类型
axios.get.mockResolvedValue({ data: 'foo' })
```

这个时候，如果用 `Mocked<typeof axios>` 来声明类型，TypeScript 就会知道 `axios.get` 已经是一个 mock 函数，可以安全调用 mock 方法。
