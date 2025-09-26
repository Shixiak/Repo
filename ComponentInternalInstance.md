# ComponentInternalInstance

## 简介

这是 Vue3 内部用来描述和管理组件实例的一个对象。

可以通过 `VNode.component` 拿到这个对象。

## 关键字段

一些核心属性如下：

### 与组件本身相关

* `vnode`: 当前组件对应的 VNode。
* `type`: 组件定义（即用户写的 `defineComponent` 配置对象或函数组件）。
* `appContext`: 当前应用上下文（`provide/inject`、全局配置等）。

### 上下文和数据

* `props`: 响应式的 props。
* `attrs`: 非 props 的 attribute。
* `slots`: 插槽内容。
* `setupState`: `setup()` 返回的对象。
* `ctx`: 组件代理对象（`this` 指向的那个）。

### 渲染相关

* `render`: 渲染函数。
* `subTree`: 上一次渲染生成的 VNode 树。
* `next`: 下一次要渲染的 VNode。

### 生命周期相关

* `isMounted`: 是否已经挂载。
* `isUnmounted`: 是否已卸载。
* `bm / m / bu / u`: 分别对应 beforeMount、mounted、beforeUpdate、updated 等钩子队列。

### 依赖追踪与更新

* `update`: 触发组件更新的函数（scheduler 调度）。
* `renderEffect`: 组件渲染副作用。
* `effect`: 响应式副作用实例（ReactiveEffect）。

### 组件关系

* `parent`: 父组件实例。
* `provides`: `provide` 提供的依赖。
* `exposed`: `expose()` 暴露给父组件的实例。
