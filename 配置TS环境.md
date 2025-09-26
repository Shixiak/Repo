# 配置 TS 环境

## 最小 TS 环境

这个环境适用于纯手写 ts 代码，不调用外部的 node 包。

创建目录结构如下：

```bash
ts-demo
├── dist/
├── src/
│   └── index.ts
├── index.html
└── tsconfig.json
```

配置 `tsconfig.json` 如下：

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "esnext",
    "sourceMap": true,
    "strict": true,
    "outDir": "dist"
  },
  "include": ["src/**/*.ts"]
}
```

在 `index.html` 引用编译的 js 文件：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>TS Demo</title>
</head>
<body>
  <h1>TS-Playground</h1>
  <script src="./dist/index.js"></script>
</body>
</html>
```

最后使用 `live server` 或者其他什么扩展打开即可。


## 手动方法

### 初始化 node 包环境

执行命令

```bash
pnpm init
```

命令执行完成后，当前目录下会多出一个 package.json 的文件。

### 安装 typescript

安装 typescript。

```bash
pnpm i -D typescript
```

安装完成之后可以使用命令验证：

```bash
$ pnpm exec tsc -v
Version 5.8.3
```

这条命令调用的是项目中的 typescript。

### 初始化 tsconfig

运行命令初始化 tsconfig

```bash
pnpm exec tsc --init
```

在生成的 tsconfig.json 中填入以下内容：

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "sourceMap": true   // 支持调试
  },
  "include": ["src"]
}
```

### 创建目录

在项目根目录下创建 src 目录和 dist 目录。

### 配置 VSCode 调试环境

#### 配置 lanch.json

创建 .vscode/lanch.json，并写入以下内容：

```bash
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Run TypeScript with pnpm",
      "preLaunchTask": "build-ts",
      "program": "${workspaceFolder}/dist/index.js",
      "outFiles": ["${workspaceFolder}/dist/**/*.js"]
    }
  ]
}
```

### 配置 tasks.json

创建 tasks.json，并写入以下内容：

```bash
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build-ts",
      "type": "shell",
      "command": "pnpm exec tsc",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "problemMatcher": ["$tsc"]
    }
  ]
}
```

