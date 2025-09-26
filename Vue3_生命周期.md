# Vue3 生命周期

在 Vue 3 里，组件的生命周期做了一次简化和改造，既保留了 Vue 2 的大部分概念，又把一些阶段合并到了 `setup()`。我帮你系统梳理一下：

## Vue 3 的完整生命周期钩子

### 创建阶段

`setup(props, context)`

* Vue 3 新增的核心入口，替代了 Vue 2 的 `beforeCreate` 和 `created`。
* 在这里初始化数据、计算属性、watch 等。
* 注意：此时 `this` 还不可用。

### 挂载阶段

`onBeforeMount`: 组件挂载到 DOM 前调用。
`onMounted`: 组件挂载完成后调用，DOM 已经生成，常用于操作 DOM 或发起请求。

### 更新阶段

`onBeforeUpdate`: 响应式状态变更，导致更新时，DOM 打补丁之前调用。
`onUpdated`: 响应式状态变更并完成 DOM 更新后调用。

### 卸载阶段

`onBeforeUnmount`: 组件卸载前调用。
`onUnmounted`: 组件卸载后调用。

### 错误处理

`onErrorCaptured(err, instance, info)`: 当子组件抛出错误时触发，可以捕获并处理错误。

### 调试钩子（开发环境用）

`onRenderTracked`: 响应式依赖被追踪时调用。
`onRenderTriggered`: 响应式依赖触发更新时调用。
