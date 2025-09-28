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

## list 的行为

```bash
nvm list
->     v22.17.0
default -> 22.17.0 (-> v22.17.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v22.17.0) (default)
stable -> 22.17 (-> v22.17.0) (default)
lts/* -> lts/jod (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.2 (-> N/A)
lts/hydrogen -> v18.20.8 (-> N/A)
lts/iron -> v20.19.5 (-> N/A)
lts/jod -> v22.20.0 (-> N/A)
```

* `->` 指向我当前的版本
* `lts/argon` 之类的是别名, 我可以使用 install 快速安装.
* `-> N/A` 说明没有安装在我的本地.

## 全局模块隔离

使用不同的 node 版本下载的全局模块位于不同的目录下。
