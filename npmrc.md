# npmrc

## 简介

`.npmrc` 是 npm/pnpm/yarn 都会识别的配置文件, 用来配置包管理的行为.

## force-legacy-deploy

pnpm deploy 的两种实现

默认模式: 不配置 `force-legacy-deploy = true`, deploy 会基于 workspace 根目录的 `pnpm-lock.yaml`, 根据目标子包的 `package.json 裁剪掉不需要的依赖, 生成一个专用的 lockfile.

legacy 模式: deploy 不会生成新的 lockfile, 而是沿用根目录的 lockfile, 把依赖拷过来, 带过去的依赖可能比实际需要的更多, 不够干净.

## shamefully-hoist

默认情况下 pnpm 只会在顶层放 package.json 直接声明的依赖的 symlink, 当时当我们拷贝 deploy 裁剪产物的时候, 会导致 symlink 失效. 就会出现这样一种情况, express 依赖了 body-parser, 并且 express 在自己的 package.json 中将 body-parser 表明成了依赖, 正常情况下:

1. express 内部 require body-parser.
2. pnpm 通过 node_modules 顶层 express 的 symlink 去 .pnpm 里面找 <express@x.y.z>.
3. pnpm 再通过这个 express 内部的 node_modules 找到 body-parser 的 symlink
4. 最后通过 body-parser 的 symlink 找到 .pnpm 下面的 <body-parser@x.y.z>

但是复制裁剪产物之后, symlink 就失效了, express 是直接在子包 package.json 里面声明的依赖, pnpm 会在 node_module 的顶层声明它, 但是它不再是一个 symlink, 也就无法再指向 .pnpm 里面的 <express@x.y.z>, 在 express 内部 require body-parser 的时候也就无法找到了.

但是我们可以通过配置 `shamefully-hoist = true` 来把所有的子依赖都提升到 node_moudle 目录下, 这样即便 symlink 被破坏了, body-parser 也能够被 express 找到, 因为 body-parser 已经在顶层了.
