# v-model 原理与实现

## 简介

v-model 其实是 v-bind 和各种事件绑定的语法糖。

由于原生 dom 改变 value 时触发的事件各有不同，所以需要在不同的事件上绑定重新赋值的函数。

input 的 dom 的 v-model 语法糖和编译后代码对照如下：

```html
<input 
  :value="foo"
  @input="($event) => foo=$event.target.value"
>
<input 
  type="checkbox"
  :checked="bar"
  @change="($event) => bar=$event.target.checked"
>
...
```

在 vue3 中自定义组件上的 `v-model` 语法糖等价于：

```html
v-model="foo"  
⇔  
:modelValue="foo" @update:modelValue="val => foo = val"
```

可以看到自定义组件重新给 value 赋值的函数默认绑定在了 `update:modelValue` 这个事件上，所以我们只需要在自定义组件更新 value 时，触发一下这个事件就行了。

让我们来看看自定义组件的 v-model 如何实现的？

MyInput.vue

```html
<template>
  <input type="text" :value="modelValue" @input="handleInput">
</template>


<script setup lang="ts">
defineProps<{
  modelValue: string
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>();

function handleInput(event) {
  emit('update:modelValue', event.target.value);
}
</script>
```

App.vue

```html
<template>
  <dz-input v-model="inputText"></dz-input>
  <p>{{ inputText }}</p>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import DzInput from './components/DzInput.vue';
const inputText = ref('');
</script>
```

当我们修改 input 输入框里面的内容的时候，会触发 handleInput，由于这是 input 的原生事件，所以在调用函数的时候会传入 event，在 handleInput 这个函数内部会使用 emit 触发 `update:modelValue`，同时传递参数 `event.target.value`，而我们在外部又使用了 v-model，这个也就是为 `update:modelValue` 绑定好了函数。也就是会把新值赋给 modelValue。

## 要点总结

v-model 写在原生 dom 上和自定义组件上时，会编译成不同的代码。甚至写在不同的原生 dom 上时都会编译成不同的代码。
