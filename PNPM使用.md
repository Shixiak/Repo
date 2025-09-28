# PNPM 使用

## 查看全局路径

使用以下命令查看全局路径

```bash
pnpm root -g
```

## 配置全局目录

使用 `pnpm setup` 自动配置 PNPM_PATH，Path 等相关环境变量，配置完成之后重启系统。

重启完成之后就可以在全局系统安装全局工具了。

## 执行命令

### 已安装

`pnpm exec`：执行已安装依赖包的二进制命令，如果项目中没有安装对应的依赖，会报错。

`pnpm <binary>`pnpm 先尝试当成脚本执行，找不到脚本后就自动退回去调用 `pnpm exec`.

### 临时执行

`pnpm dlx`：等价 npx，可以执行未安装的包，会临时下载包并运行。

## 构建脚本

```bash
Warning
Ignored build scripts: esbuild.
Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.
```

这是 pnpm v8+ 引入的新的安全机制：pnpm 默认不在自动执行依赖中的构建脚本（build scripts），以防止潜在的恶意代码执行。

## deploy

deploy 是 pnpm 提供的一个产物裁剪工具.

它的作用是从一个 monorepo 中, 把某个子包挑出来, 根据该子包的 package.jjson 里面的依赖, 以及根目录的 pnpm-lock.yaml, 裁剪出一个独立可运行的目录(包含 dist, 必须的 node_modules), 用来直接部署到服务器.
