# Watch

## 监听单个数据

watch 的函数签名

```ts
watch(source: WatchSource<unknown>, cb: WatchCallback<unknown, unknown>, options?: WatchOptions<false> | undefined): WatchHandle
```

有两个要注意的点：

* 如果 source 是 Ref 的话，vue 会自动解包它，也就是说 `watch(count, cb)` 等价于 `watch(() => count.value, cb)`。
* 因此，我们在回调中拿到的 newVal 和 oldVal 实际上也是解包后的值。

要监听单个数据，可以直接传入这个变量和变量变化时要执行的回调就行了。比如下面这段代码，直接在 watch 中传入 count 和一个回调函数，这个回调函数有两个参数，第一个参数是 newVal，第二个参数是 oldVal。

```vue
<template>
  <p>count = {{ count }}</p>
  <button @click="count++">count++</button>
</template>


<script setup lang="ts">
import { ref, watch } from 'vue';

const count = ref(0);
watch(count, (newVal, oldValue) => {
  console.log(`count changed from ${oldValue} to ${newVal}`)
})
</script>
```

但是还有一个问题，如果我的代码是这个样子：

```vue
<template>
  <p>{{ nums }}</p>
  <button @click="nums.push(nums.length + 1)">push</button>
</template>


<script setup lang="ts">
import { ref, watch } from 'vue';

const nums = ref<number[]>([]);

watch(nums, (newVal, oldVal) => {
  console.log('oldVal: ', oldVal);
  console.log('newVal: ', newVal);
})
</script>
```

侦听就会失效，因为 watch 默认只会对 ref 的 `.value` 的引用变化进行检测。这里 nums.value 指向始终是最开始的那个数组，引用没有变化过，变化的只是数组里面的元素，所以回调没有被调用。

如果想要回调被触发，就需要开启深度侦听，

```vue
watch(nums, (newVal, oldVal) => {
  console.log('oldVal: ', oldVal);
  console.log('newVal: ', newVal);
}, {
  deep: true
})
```

但是深度侦听也是有问题的，拿到的始终是最新的 nums，因为 nums.value 的应用从来没有变过。这里还有一个细节，就是因为我想打印最新的数组，nums.value 指向的数组一定是更新过后的。但是 oldVal 和 newVal 实际上都是 nums.value，他们是同一个引用，所以拿不到旧值。

## 监听多个数据

和监听单个数据其实很相似。传入 watch 的参数形式有所变化。第一个参数变成了一个列表，第二个参数还是回调函数，但是回调函数的第一个参数是新值的列表，第二个参数是旧值的列表。

```vue
<template>
  <div class="container">
    <p>a: {{ a }}</p>
    <p>b: {{ b }}</p>
  </div>
  <hr>
  <button @click="a+=1; b+=2">a+1, b+2</button>
</template>


<script setup lang="ts">
import { ref, watch } from 'vue';

const a = ref(0);
const b = ref(0);

watch([a, b], ([newValA, newValB], [oldValA, oldValB]) => {
  console.log(`a changed from ${oldValA} to ${newValA}`);
  console.log(`b changed from ${oldValB} to ${newValB}`);
});
</script>
```

## 监听 getter

直接侦听 reactive 对象时拿不到旧值，要侦听属性的变化需要使用 getter 函数。写法就是将第一个参数携程箭头函数，返回值是要侦听的属性。

```vue
<template>
  <div>
    <h3>p</h3>
    <p>{{ p.name }}</p>
    <p>{{ p.age }}</p>
  </div>
  <hr>
  <button @click="p.age++">age+1</button>
</template>


<script setup lang="ts">
import { reactive, watch } from 'vue';

const p = reactive({
  name: 'Alice',
  age: 18
})

watch(() => p.age, (newVal, oldVal) => {
  console.log('oldVal: ', oldVal); 
  console.log('newVal: ', newVal);
});
</script>
```

## 侦听时机

`watch` 里的 `flush` 是 Vue 3 给 `watch` / `watchEffect` 提供的一个**执行时机控制选项**，用来决定 **侦听回调** 在组件更新流程中的哪一步执行。

### 语法

```ts
watch(source, callback, {
  flush?: 'pre' | 'post' | 'sync'  // 默认是 'pre'
})
```

### 三种模式的区别

| 值        | 执行时机                    | 典型用途                                      |
| -------- | ----------------------- | ----------------------------------------- |
| `'pre'`  | **组件更新前**（DOM 还没更新）     | 默认值；适合在数据变了就立刻跑逻辑（比如计算衍生状态、触发副作用），不依赖 DOM |
| `'post'` | **组件更新后**（DOM 已更新）      | 当回调需要读取**最新 DOM**（比如测量元素尺寸、调用 Popper 定位）时 |
| `'sync'` | **同步**（数据变就马上跑，不等下一轮更新） | 调试、强制同步逻辑、和第三方非 Vue 响应系统交互时               |
