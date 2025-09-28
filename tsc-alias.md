# tsc-alias

## 简介

一个开发依赖, 用于将编译后的 js 文件中的 路径别名替换成正确的相对路径.

用于只使用 tsc 编译, 而不是用 vite/webpack 等打包工具的场景.

## 配置脚本

```json
scripts: {
  "build": "tsc && tsc-alias"
}
```
