# @ 的含义

## 简介

`@` 前缀表示**包处于一个命名空间（scope）里**，而没有 `@` 的是**普通（顶级）包**。

## **无 @ 的包**

直接发布到 npm 根空间（registry 的顶级命名空间）。

例如：

```text
lodash
vue
express
```

安装方式：

```bash
npm install lodash
```

## 有 @ 的包（Scoped Packages）

形如 `@scopeName/packageName`。

`scopeName` 是**组织名**或**用户 npm 账号名**，相当于一个命名空间。

例如：

```text
@vue/runtime-core
@element-plus/icons-vue
@babel/core
```

安装方式：

```bash
npm install @vue/runtime-core
```
