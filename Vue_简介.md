# Vue

## Vue 基础与核心

### Vue 的基本原理

当一个 Vue 实例创建的时候，Vue 会遍历 data 中的属性，并使用 Object.defineProperty（Vue3 中使用 Proxy）将数据转换为 getter/setter，这样就能够监听和拦截对数据的访问和修改。每个组件实例都有对应的 watcher 程序实例，它们会在组件渲染过程中把属性记录为依赖，之后当依赖项的 setter 被调用的时候，会通知 watcher 重新计算，从而致使它关联的组件及时更新。

![vue_theory](https://kat-tc.oss-cn-chengdu.aliyuncs.com/img/20250602214448.png)

## 编译宏

编译宏，只能在 `<script setup>` 里面使用，并且只能在文件顶层调用。

### defineProps

`defineProps()` 是 Vue3 中的编译宏（compile-time macro），用于在组件中声明并捕获 props 属性。

三种使用方式：

传入列表（即将弃用）：

```html
<script setup>
defineProps(['title', 'count']);
</script>
```

显示传入类型定义：

```html
<script setup lang="ts">
interface Props {
  title: string,
  count: number
}
defineProps<Props>();
</script>
```

传入参数对象：

```html
<script setup lang="ts">
defineProps({
  title: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    default: 0
  }
});
</script>
```

### defineEmits

这个宏可以用来定义自定义事件，

有两种定义自定义事件的方法，比较基础的用法是下面这样的

```ts
const emit = defineEmits(['custom-event']);
```

这样定义之后，我们可以使用 `emit('custom-event', 'hello from dz-button', 123)` 这种方式来触发事件并传递参数，但是这样有一个很明显的问题，就是我在写参数的时候比较随意，类型不确定，个数不确定。这在我们 ts 怎么能够允许呢？于是就有了下面这种写法：

```ts
const emit = defineEmits<{
  (e: 'custom-event', msg: string, num: number): void
  (e: 'another-event'): void
}>()
emit('custom-event', 'hello from dz-button', 123);
```

这里面，e 就是一个 `'custom-event' | 'another-event'` 联合类型。括号里面定义的格式，决定了调用 emit 是参数的格式。

### defineExpose

向父组件暴露方法。

### defineOptions

定义父组件的配置。

### withDefault

给 `defineOptions` 提供默认值。

## 响应式与监听

[Watch](./Watch.md);

## 组件间常用的通信方式有哪些？

### props/$emit

两种方式的常见使用场景：

* props：父组件向子组件传递数据，既可以传递数据也可以传递函数。子组件通过 props 接收数据。注：组件中的数据共有三种形式：data, computed, props。
* $emit：父组件为子组件绑定自定义事件，并给该自定义事件绑定一个函数，子组件触发该事件时，将数据传递到该函数中。

介绍一下 Vue3 中传递参数的方法：

首先需要在组件中使用 `defineProps` 定义需要接受的参数，这个定义包含了参数的类型，是否必须等。定义之后可以直接在模板中使用。如果需要在 setup 里面使用，就需要使用一个变量接受 `defineProps` 的返回值。

```html
<template>
  <!-- 模板里面可以直接拿到 myType -->
  <button :my-type="myType">
    <span>
      <slot></slot>
    </span>
  </button>
</template>


<script setup lang="ts">
const props = defineProps({
  myType: {
    type: String,
    default: 'primary'
  }
})
console.log(props);
</script>
```

然后在 app.vue 里面向组件传入对应的参数。如果在传入参数时，参数名写的是 `my-type` 这种带有短横线的写法，则在定义的时候需要写 `myType`。

```html
<template>
  <dz-button my-type="primary">测试</dz-button>
</template>


<script setup lang="ts">
import DzButton from './components/DzButton/DzButton.vue';
</script>
```

### 事件总线

`$bus`是 Vue 组件间一种轻量级的通信方式，适用于非父子组件之间的数据通信与事件通知。通过一个 Vue 实例作为中央事件管理器。要接收数据的组件首先使用 `$on` 创建自定义事件并绑定回调，要发送消息的组件使用 `$emit` 触发事件并传递数据。在 Vue2 中非常常见，但是在大型项目或者 Vue3 中建议使用更清晰的状态管理和事件库。

### Vuex

Vuex 通过创建一个全局的 store 实例，集中管理所有组件共享的状态（state），它充当了组件间共享数据的中间桥梁。组件要获取数据可以通过 state 获取，组件要修改数据可以通过 commit 直接修改或者 dispatch 让 action 调用 commit 简介修改数据。

### project\inject

基本原理：

* `provide` 在祖先组件中提供一些数据，形式是键值对。
* `inject` 在后代组件中通过注入的方式拿到数据。

举一个例子，这段代码通过 provide 和 inject 实现里数据的传递。

App.vueApp.vue，这里设计了一个方法来改变 count，用来判断 DzCard 拿到的 count 到底是不是响应式的。

```html
<template>
  <dz-card></dz-card>
  <hr>
  <button @click="count++">count++</button>
</template>


<script setup lang="ts">
import { provide, ref } from 'vue';
import DzCard from './components/DzCard/DzCard.vue';
let count = ref(0);
provide('count', count);
</script>
```

DzCard.vue 这里的 count 会随着按钮的点击而变化，说明 DzCard 拿到的是响应式的对象。

```html
<template>
  <div class="dz-card">
    <h2>DzCard</h2>
    <p>count = {{ count }}</p>
  </div>
</template>


<script setup lang="ts">
import { inject } from 'vue';

let count = inject('count');
</script>
```

虽然代码能够正常运行，但是实际在编写的过程中有两个问题

* 向 inject 传入 count 的时候没有提示，容易敲错代码。
* 拿到的 count 类型为 unknown。

这个时候 InjectionKey 就排上用场了。首先 InjectionKey 本质上还是在限定 Key 的类型。就比如下面这个 CountKey，他的本质能是一个 Symbol 类型的变量。InjectionKey 本质上也是对 Symbol 的泛型封装。这里传入的 `InjectionKey<Ref<number>>`，我们一层一层来解读。`InjectionKey<T>` 表示这个 key 对应的 value 的类型是 T，这里就是说 CountKey 对应的 value 的类型是 `Ref<number>`。`Ref<number>` 的意思就是，这个响应式对象的 value 的类型是 number。

```ts
export const CountKey: InjectionKey<Ref<number>> = Symbol('CountKey');
```

### \$parent/\$children 与 ref

使用这几个 API 可以获取目标组件的实例，然后通过 `.` 运算符直接拿到它们身上的属性和方法：

* `$parent` 可以获取当前组件的父组件，可以调用父组件的方法和获取父组件的数据。
* `$children` 可以获取子组件（无法跨层）的列表。`$children` 是无序的数组，组件顺序可能不可靠。
* `ref` 可以用于给组件设置引用，标记之后通过 `this.$refs.ref` 可以精确的获取组件的实例。`ref` 也可以用于原生 DOM 元素。

### 插槽实现通信

通过插槽也能够实现组件间通信，子组件可以通过在插槽上绑定数据，父组件就能使用 v-slot 收到数据，并控制数据渲染的格式。

## 运行时 API

### useSlots

获取 slots。

### useAttrs

获取透传属性。

## 接口拓展点

### GlobalComponents

Vue 的 `@vue/runtime-core` 包中的一部分，是专门留给开发者通过模块增强添加全局组件类型的一个接口。

用例：

```ts
import type { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

declare module 'vue' {
  export interface GlobalComponents {
    FontAwesomeIcon: typeof FontAwesomeIcon;
  }
}
```
