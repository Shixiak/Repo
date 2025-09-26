# Vitest

Vitest 是一个现代化、Vite 原生支持的测试框架，支持 TypeScript、ESM、Vue、React 等项目。

## 配置

我们假设已经有了一个初始的 vue3+ts 的项目。并以此为基础配置 vitest。

安装 vitest

安装 vitest 为开发依赖：

```bash
pnpm i vitest -D
```

安装 jsdom

如果需要 DOM 环境（测试 Vue，React 组件时）,还需要安装 jsdom。

```bash
pnpm i jsdom -D
```

配置 vite.config.ts

vitest 会复用 vite 的配置，所以我们在配置头部加上 `/// <reference types="vitest" />` 然后在配置对象中加上 test 选项。

```ts
// vite.config.ts
/// <reference types="vitest" />
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  test: {
    globals: true, // 允许 describe/test 直接使用，不用 import
    environment: 'jsdom', // 浏览器环境
  },
})
```

`globals: true`

自动注入全局测试 API（`describe`、`it`/`test`、`expect`、`beforeEach`...）

不用再每个测试文件手动：

```ts
import { describe, test, expect } from 'vitest'
```

`environment: 'jsdom'`

* 指定测试运行环境为 **jsdom**（虚拟浏览器环境）。
* 提供 `window`、`document`、`HTMLElement` 等 DOM API。
* 必须在测试 Vue/React 组件时使用（Node 环境默认没有这些 API）。
* 其他可选值：
  * `'node'`：纯 Node.js 环境（默认值）
  * `'happy-dom'`：另一种轻量的浏览器环境模拟器

在 `/src` 目录下新建一个测试文件 demo.test.ts

```ts
import { test, expect } from 'vitest'

test('basic', () => {
  expect(1 + 1).toBe(2);
})
```

运行测试

```bash
pnpm exec vitest demo
```

## 测试结构

### describe

测试套件（test suite），逻辑分组工具，用来组织一组相关的测试用例。

### 使用方法

#### 模块全局 mock

```ts
import axios from 'axios';
import { vi, Mocked } from 'vitest';

vi.mock('axios');

const mockedAxios = axios as Mocked<typeof axios>;

mockedAxios.get.mockResolvedValue({ data: 'mocked' });
```

这样 TS 不会报错，并且有智能提示：

```ts
mockResolvedValue
mockImplementation
mockReturnValue
```

#### 局部对象 mock

```ts
interface UserService {
  getUser: (id: string) => Promise<string>;
  saveUser: (name: string) => Promise<void>;
}

const service: Mocked<UserService> = {
  getUser: vi.fn(),
  saveUser: vi.fn()
};

service.getUser.mockResolvedValue('Alice');
```

这里的 `service` 已经是“mock 化的接口”，方便测试时手动构造。

## 断言

### 检查是否存在样式

```ts
expect(wrapper.element.style.color).toBe('red');
```
