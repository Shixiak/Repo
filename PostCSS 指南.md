# PostCSS

## 简介

PostCSS 是一个基于 JavaScript 的 CSS 转换工具平台。它不是一个预处理器，而是一个插件运行平台。

## 配置

创建配置文件 `postcss.config.js`，以配置 `postcss-each` 为例。

```js
import each from 'postcss-each';

export default {
    plugins: [
        each()
    ]
}
```

`vite.config.ts` 中也可以配置 `postcss`：

```ts
import each from 'postcss-each';

// https://vite.dev/config/
export default defineConfig({
  // ...
  css: {
    postcss: {
      plugins: [
        each()
      ]
    }
  }
})
```
