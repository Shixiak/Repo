# ESLint

## 配置过程

最简配置过程：

执行命令：

```bash
pnpm create @eslint/config@latest
```

跟随引导选择具体的配置，选择完成之后下载依赖。

在 eslint.config.js 里面添加规则：

```js
import { defineConfig } from "eslint/config";

export default defineConfig([
 {
  rules: {
   "no-unused-vars": "warn",
   "no-undef": "warn",
  },
 },
]);
```

然后启用 VSCode 扩展即可。
