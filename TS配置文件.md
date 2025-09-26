# TypeScript

## tsconfig 配置

### CompilerOptions

#### types

用来引入类型。

示例：

```json
// tsconfig.json
{
  "compilerOptions": {
    "types": ["element-plus/global"]
  }
}
```

这里面的 `element-plus` 是我下载包的名字。

#### allowImportingTsExtensions

![image-20250721105500958](https://kat-tc.oss-cn-chengdu.aliyuncs.com/undefinedimage-20250721105500958.png)

这个错误出现的原因是默认情况下，ts 不允许在 import 中显式写 `.ts` 后缀，除非在 `tsconfig.json` 中配置了 `allowImportingTsExtensions: true` 。但是不建议配置，因为这样配置过后，编译成 js 代码还得手动修改对应代码回 js。

#### baseUrl 和 path

模块导入的基础目录，会影响非相对目录的解析。

比如我当前的 `tsconfig.json` 配置是这样：

```ts
{
  "compilerOptions": {
    "baseUrl":"src",
    "paths":{
      "@/*": ["src/*"]
    }
  }
}
```

那我在导入模块时这两种写法就会受到影响：

* `import xxx from 'api'` ，ts 编译器就会去 `project-root\src\` 去找这 api 这个文件。
* `import xxx from '@/api'`，ts 编译器就会去 `project-root\src\src\` 去找 api 这个文件。

## 类型系统

### interface

可以拓展：

```ts
interface User {
    token: string,
    password: string,
    email: string
}

interface User {
    nickname: string
}
```

### type

可以使用 Record。

#### Record

示例代码

```ts
interface User {
    token: string,
    password: string,
    email: string
}

export type Users = Record<string, User>;
```

Record 这行代码的含义：一个对象，它的键可以是任意 string，每个键对应的值的类型必须是 User。

Record 是一个类型工具，只能通过 type 定义。

## 模块增强

`declare module` 对已有模块进行类型增强或者声明定义。常用于：

1. 为第三方库添加类型信息
2. 扩展现有的模块类型（比如 Vue 的全局组件声明）
3. 声明模块而不引入真实的代码实现

全局组件声明：

```ts
// src/globalComponents.d.ts
import type { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

declare module 'vue' {
  export interface GlobalComponents {
    FontAwesomeIcon: typeof FontAwesomeIcon;
  }
}
```

## 三斜杠指令

### 引入额外类型声明

```ts
/// <reference types="vitest" />
```

TypeScript 的三斜杠指令（triple-slash directive）的一种，用于在当前文件中额外引入 `vitest` 这个包的类型声明。

### 全局替代方案

另一种替代方案，可以在 `tsconfig.json` 里面全局声明：

```json
{
  "compilerOptions": {
    "types": ["vitest"]
  }
}
```
