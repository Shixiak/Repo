# 模板中的 Ref

## 参数传递

传递参数给子组件

父组件中：

```vue
<Child :msg="myRef" />
```

```ts
const myRef = ref('hello')
```

子组件中：

```ts
defineProps<{ msg: Ref<string> }>()
// msg.value === 'hello'
```

## 自动解包

Vue 模板自动解包 ref 只在模板表达式（`{{}}` 或 HTML 标签模板中生效）。
