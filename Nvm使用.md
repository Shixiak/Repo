# Nvm 使用

本文简单简单介绍一下 nvm 的安装和使用。

## 安装nvm

非常简单，打开 [nvm 官网](https://github.com/coreybutler/nvm-windows/)下载安装即可。安装完成后可以通过在 cmd 输入命令 `nvm` 来验证是否安装成功。

```cmd
C:\Users\DZZI>nvm

Running version 1.2.2.

Usage:
  ...
```

## 安装 node

安装不同版本的 node。

### LTS 版本

安装 lts 版本的 node.js，命令如下：

```cmd
nvm install --lts
```

lts 是参数，所以要加 `--`。

### 具体版本

安装具体版本的 node 的命令如下：

```cmd
nvm install 22.17.0
```

## 全局模块隔离

使用不同的 node 版本下载的全局模块位于不同的目录下。
