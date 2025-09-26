# @vue/test-utils

## 概述

* **类别**：Vue 官方测试工具库
* **作用**：负责**挂载（mount）Vue 组件**，模拟用户交互，获取组件状态。
* **核心功能**：

  * `mount()` / `shallowMount()` 挂载组件
  * `wrapper.find()`、`wrapper.text()` 等 DOM 查询方法
  * `wrapper.emitted()` 获取组件 emit 的事件
  * 模拟事件：`wrapper.trigger('click')`
  * 测试组件状态和渲染结果

## 挂载方法

### mount

`mount(DzButton)` 返回的是一个 `VueWrapper` 实例，内部封装了组件实例。

#### 简介

`mount()` 用来**在测试环境中完整渲染（挂载）一个 Vue 组件**，并返回一个 `VueWrapper` 实例，方便程序员：

* 查询 DOM
* 模拟用户事件
* 检查组件状态
* 获取组件触发的自定义事件
* 修改 props / data 等

如果你只需要**部分挂载**（不渲染子组件），可以用 `shallowMount()`。

#### 基本语法

```ts
import { mount } from '@vue/test-utils'

const wrapper = mount(Component, options?)
```

* **`Component`**：要挂载的 Vue 组件（可以是 `.vue` 文件或对象）
* **`options`**（可选）：挂载时的配置项

#### 常用配置项

| 配置项    | 说明             |
| --------------------- |--------------------------------- |
| `props` / `propsData` | 给组件传递 props（Vue 3 用 `props`，Vue 2 用 `propsData`） |
| `slots`               | 定义插槽内容    |
| `attrs`               | 定义 HTML attribute        |
| `global`     | 全局配置（比如全局组件、插件、mocks、provide、stubs） |
| `data()`              | 初始化组件 data        |
| `attachTo`            | 指定挂载的 DOM 容器（默认在 jsdom 内存中）    |

#### 示例

```ts
mount(MyButton, {
  props: { label: 'Click me' },
  slots: { default: '<span>Slot content</span>' },
  global: {
    components: { MyIcon },
    provide: { theme: 'dark' },
    stubs: ['RouterLink'] // 替换 RouterLink 为 stub
  }
})
```

### shallowMount

## 全局配置 Global

### stubs

[stubs](./stubs.md)
