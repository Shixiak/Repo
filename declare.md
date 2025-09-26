# declare

## 作用

比如一个没有类型定义的第三方库 `legacy-utils`：

```ts
// index.ts
import legacy from 'legacy-utils'

legacy.doSomethingCool()
```

这时 TypeScript 会报 TS7016。

```text
TS7016: Could not find a declaration file for module 'legacy-utils'
```

解决办法：写个 `.d.ts` 文件：

```ts
declare module 'legacy-utils' {
  export function doSomethingCool(): void
  export function sum(a: number, b: number): number
}
```

再写代码：

```ts
import { sum } from 'legacy-utils'

console.log(sum(1, 2))  // 出现类型提示
```
