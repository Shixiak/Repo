# Node 使用

## 启用 pnpm

从 Node.js v16.13 起，Node 官方内置了一个包管理桥接工具叫 `corepack`，它可以快速启动 `pnpm` 和 `yarn` 等包管理工具。

启用的命令：

```bash
corepack enable
corepack prepare pnpm@latest --activate
```

启用之后可以通过 `pnpm -v` 验证 pnpm 是否启用。

## tsconfig-paths

一个帮助 ts-node 识别路径别名的工具。

需要在 package.json 里面修改命令

```json
{
  "scripts": {
    "dev": "ts-node -r tsconfig-paths/register src/app.ts"
  }
}
```
