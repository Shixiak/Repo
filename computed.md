# computed

## 简介

在 Vue 中，`computed`（计算属性）是一个非常常用的功能，它可以根据已有的响应式数据，声明式地计算出一个新的值，并且具备缓存特性（只有依赖的数据发生变化时才会重新计算）。

## 基本使用

在 `setup` 语法糖中（Vue 3 推荐写法）：

```ts
<script setup lang="ts">
import { ref, computed } from 'vue'

const firstName = ref('Xian')
const lastName = ref('Shi')

// 使用 computed 定义计算属性
const fullName = computed(() => {
  return firstName.value + ' ' + lastName.value
})
</script>

<template>
  <p>{{ fullName }}</p>
</template>
```

这里 `fullName` 会依赖 `firstName` 和 `lastName`，只要这两个任意一个发生改变，`fullName` 就会自动更新。

---

## computed 的缓存机制

和普通函数不同，`computed` 内部有缓存：

* 第一次访问时会执行计算函数并缓存结果。
* 之后访问时，如果依赖没有变动，会直接返回缓存值。
* 依赖发生变化时，才会重新执行计算。

这比 `methods` 的好处是：如果同一个值在模板中被多次使用，不会重复计算，性能更好。

---

## 带 getter 和 setter

除了只读的 `computed`，还可以给它定义 `getter` 和 `setter`，实现**双向绑定**：

```ts
<script setup lang="ts">
import { ref, computed } from 'vue'

const firstName = ref('Xian')
const lastName = ref('Shi')

const fullName = computed({
  get() {
    return firstName.value + ' ' + lastName.value
  },
  set(newValue) {
    const [first, last] = newValue.split(' ')
    firstName.value = first
    lastName.value = last
  }
})
</script>

<template>
  <input v-model="fullName" />
</template>
```

这样在输入框里修改 `fullName`，`firstName` 和 `lastName` 也会自动更新。

---

## 和 watch 的区别

* `computed` 更适合根据已有数据**声明式地派生出新数据**；
* `watch` 更适合**监听某些数据变化后执行副作用（异步请求、打印日志、DOM 操作等）**。

## Vue 2 中的写法

在 Vue 2 中（Options API），`computed` 定义在组件选项里：

```js
export default {
  data() {
    return {
      firstName: 'Xian',
      lastName: 'Shi'
    }
  },
  computed: {
    fullName() {
      return this.firstName + ' ' + this.lastName
    }
  }
}
```
