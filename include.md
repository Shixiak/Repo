# include

作用：定义“本项目的源文件集合”。这些文件会被 TypeScript 解析、检查、参与类型合并。

* 可匹配 **具体文件** 和 **通配符**（如 `src/**/*.ts`、`types/**/*.d.ts`）。
* 只要 `.d.ts` 被纳入 `include`，其中的 ambient 声明（`declare module`、`declare global` 等）就会对同一 program 中的其它文件可见。
