# VueWrapper

`VueWrapper` 提供了很多方法，例如：

| 方法                   | 功能                          |
| -------------------- | --------------------------- |
| `.find(selector)`    | 查询单个 DOM 元素或子组件             |
| `.findAll(selector)` | 查询所有匹配的元素/组件                |
| `.text()`            | 获取渲染后的纯文本                   |
| `.html()`            | 获取渲染后的 HTML                 |
| `.trigger(event)`    | 触发事件（如 `'click'`、`'input'`） |
| `.setProps()`        | 动态修改 props                  |
| `.emitted()`         | 获取组件触发的自定义事件及参数             |
| `.unmount()`         | 卸载组件                        |

## text

获取渲染后的纯文本。

## emitted

`wrapper.emitted()` 用于获取当前组件在测试运行过程中通过 `this.$emit` 触发过的自定义事件及其参数。

### 基本语法

```ts
emitted(): Record<string, any[][]> | undefined
emitted(eventName: string): any[][] | undefined
```

* **无参调用**：返回一个对象，键是事件名，值是参数数组的数组
* **传入事件名**：只返回该事件对应的参数记录

### 返回值结构

假设被测组件：

```vue
<script setup>
const emit = defineEmits(['add', 'remove'])
emit('add', 1, 2)
emit('remove', 'x')
</script>
```

测试时：

```ts
const wrapper = mount(MyComponent)
console.log(wrapper.emitted())
```

输出：

```ts
{
  add:    [ [1, 2] ],
  remove: [ ['x'] ]
}
```

* `add` → 触发过 1 次，每次参数是 `1, 2`
* `remove` → 触发过 1 次，每次参数是 `'x'`

## findAll

在 Vue Test Utils（VTU）里，`wrapper.findAll()` 返回的子项不是原生 DOM 元素，也不是纯粹的 Vue 组件实例，而是 **一组 `VueWrapper` 或 `DOMWrapper` 实例**。

更精确地说：

* 如果你查找的是组件选择器（比如 `MyButton` 或 `ComponentName`），`findAll()` 里的每一项是 **`VueWrapper`**（封装了该组件的测试实例）。
* 如果你查找的是 DOM 选择器（比如 `'.btn'` 或 `'div'`），每一项是 **`DOMWrapper`**（封装了原生 DOM 节点）。

所以它的返回值类型大致是：

```ts
// Vue 3 + Vue Test Utils v2 类型定义
function findAll(selector: Selector): DOMWrapper<Element>[] | VueWrapper[]
```

## get

```ts
get(selector: Selector): VueWrapper
```

参数 Selector 可以是：

* CSS 选择器
* ref 引用名

返回匹配到的第一个 `VueWrapper`，如果没有匹配，会抛出错误。

## find

如果找不到目标，返回一个空的 VueWrapper，不会报错。

## findComponent

`findComponent` 用来在测试挂载的 Vue 组件树中，找到**符合条件的第一个组件实例**。它的作用和 `find` 类似，但 `find` 查的是 DOM 元素，而 `findComponent` 查的是**组件**。

### 基本用法

```ts
import { mount } from '@vue/test-utils'
import Parent from '@/components/Parent.vue'
import Child from '@/components/Child.vue'

const wrapper = mount(Parent)

// 查找第一个 Child 组件实例
const childWrapper = wrapper.findComponent(Child)
```

这里 `childWrapper` 返回的也是一个 **VueWrapper**，只不过它封装的是 **Child 组件实例**。

### 返回值

* 如果找到 → 返回 **VueWrapper**
* 如果找不到 → 返回一个 **ErrorWrapper**（几乎所有方法会抛异常）

```ts
const found = wrapper.findComponent(Child)
console.log(found.vm) // 组件实例（Proxy）
console.log(found.element) // 根 DOM 元素
```

## attrbutes

判断某个属性是否被定义：

```ts
expect(wrapper.attributes()).toHaveProperty('disabled');
```
