# 符号链接

## 简介

<https://zhuanlan.zhihu.com/p/609430861>

symlink 是什么

假设我安装了 body-parser@1.20.3, pnpm 在 node_modules 里面长这样

```text
node_modules/
├─ .pnpm/
│   ├─ body-parser@1.20.3/
│   │   └─ node_modules/
│   │       └─ body-parser/   ← 真实的包文件在这里
│   ├─ express@4.21.2/
│   │   └─ node_modules/
│   │       └─ express/       ← express 真正的文件在这里
│   └─ ... 其它包
│
├─ body-parser → .pnpm/body-parser@1.20.3/node_modules/body-parser
├─ express     → .pnpm/express@4.21.2/node_modules/express
└─ ... 其它包
```

.pnpm/ 里才是真正的包内容。

顶层的 body-parser 和 express 是 符号链接 (symlink)，指向 .pnpm/xxx 下的真实文件夹。

相当于顶层的 express 只是 .pnpm/express@x.y.z 的一个别名, 但是这个别名不是简单的直接展示 .pnpm/express@x.y.z 里面的所有内容, 而是分层投影, 将 .pnpm/express@x.y.z 的不同层级的内容组合成一个视图显示. 这个视图满足 pnpm 寻找依赖的最小标准.
