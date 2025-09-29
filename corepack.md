# corepack

## 简介

**corepack** 是 Node.js（≥16.13 默认开启）自带的一个包管理工具代理，它的主要作用是：

* 在执行 `pnpm` / `yarn` / `npm` 时，不直接用全局安装的版本，而是让 **corepack** 先拦截命令；
* corepack 会根据配置（比如 `package.json` 的 `packageManager` 字段，或者你手动执行的 `corepack use pnpm@X.Y.Z`）下载、缓存并调用指定版本的包管理器。

所以你的 `pnpm` 不是直接来自 `npm install -g pnpm`，而是 corepack 从它的缓存里调出来的一个版本。

## use

当你在项目目录下执行：

```bash
corepack use pnpm@10.13.1
```

它会做两件事：

1\. **本地环境切换版本**

在当前 Node 版本的 corepack 缓存里，激活 `pnpm@10.13.1` 作为默认版本（直到你切到别的版本为止）。

2\. **在项目中写入 `packageManager` 字段**

如果当前目录下有 `package.json`，它会自动在里面添加：

```json
"packageManager": "pnpm@10.13.1"
```

这是为了让其他人在拉取这个项目时，也能通过 corepack 自动使用同样的 pnpm 版本。

## 启动某个版本的 pnpm

首先启用 corepack:

```bash
corepack enable
```

然后在项目目录下执行：

```bash
corepack prepare pnpm@10.13.1 --activate
```
