# Vue3 的插件系统

## 插件的定义

插件是一个对象或函数, 如果是一个对象, 它必须暴露一个 **`install` 方法**. Vue 在调用 `app.use(plugin)` 时, 会执行这个 `install` 方法.

常见写法有两种：

### 函数式插件

```ts
function MyPlugin(app, options) {
  // app: 当前应用实例
  // options: 用户在 use() 时传递的参数
  app.config.globalProperties.$hello = () => console.log('Hello Vue3!')
}
```

### 对象式插件

```ts
const MyPlugin = {
  install(app, options) {
    app.config.globalProperties.$hello = () => console.log('Hello Vue3!')
  }
}
```

然后在入口文件使用：

```ts
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
app.use(MyPlugin, { someOption: true })
app.mount('#app')
```

---

## 插件能做什么

Vue3 插件系统非常灵活, 常见功能包括：

1. **添加全局方法/属性**

   ```ts
   app.config.globalProperties.$axios = axios
   ```

2. **注册全局组件**

   ```ts
   app.component('MyButton', MyButton)
   ```

3. **注册全局指令**

   ```ts
   app.directive('focus', {
     mounted(el) {
       el.focus()
     }
   })
   ```

4. **提供全局混入 (mixin)**

   ```ts
   app.mixin({
     created() {
       console.log('plugin mixin created')
     }
   })
   ```

5. **注入依赖 (provide/inject)**
   插件里可以用 `app.provide` 提供全局数据, 组件通过 `inject` 获取：

   ```ts
   app.provide('globalMessage', 'Hello from plugin')
   ```

---

## 插件的执行时机

* 插件通过 `app.use()` 注册, Vue 会自动调用插件的 `install` 方法.
* 插件注册顺序很重要：如果多个插件依赖同一个全局属性, 必须注意 `app.use()` 的先后顺序.
* 插件只会被安装一次, 即使多次调用 `app.use(plugin)`.

---

## 插件的设计原则

* **高内聚、低耦合**：插件应该是独立的功能模块, 不和具体项目逻辑强绑定.
* **可配置性**：尽量允许用户通过 `options` 自定义行为.
* **可复用**：写成插件后, 方便在不同项目中复用.

---

## 实战示例：封装一个 Toast 插件

```ts
// toastPlugin.ts
import ToastComponent from './Toast.vue'

export default {
  install(app) {
    const showToast = (msg: string) => {
      // 动态挂载 Toast 组件
      const div = document.createElement('div')
      document.body.appendChild(div)

      const toastApp = createApp(ToastComponent, { message: msg })
      toastApp.mount(div)
    }

    // 全局方法
    app.config.globalProperties.$toast = showToast
    // 也可以提供给 setup 中使用
    app.provide('toast', showToast)
  }
}
```

使用：

```ts
app.use(ToastPlugin)

this.$toast('Hello from plugin!')

// setup 中
const toast = inject('toast')
toast('Show in setup')
```
