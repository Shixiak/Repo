# AsyncValidator

## 简介

AsyncValidator 是 `yiminghe/async-validator` 这个库导出的一个类。它是一个独立的验证库，和 Vue 没有直接关系。作用就是：根据规则（rules）去验证一个 对象（values），支持异步。

Element-Plus、Ant Design Vue、iview 等 UI 库的表单验证，底层都是基于这个库的。

## 示例

**定义规则**：规则是一个对象，key 对应字段名，值是规则数组：

```ts
import AsyncValidator from 'async-validator'

const descriptor = {
  username: [
    { required: true, message: '用户名不能为空' },
    { min: 3, max: 10, message: '用户名长度在3~10之间' }
  ],
  age: [
    { type: 'number', min: 18, message: '必须年满18岁' }
  ]
}
```

**创建验证器：**

```ts
const validator = new AsyncValidator(descriptor)
```

**传入数据进行验证：**

```ts
validator.validate({ username: 'Tom', age: 15 })
  .then(() => {
    console.log('验证通过')
  })
  .catch(err => {
    console.log('验证失败:', err.errors, err.fields)
  })
```

**返回结果：**

* 成功 → resolve（进入 `.then`）
* 失败 → reject，错误信息格式：

  ```ts
  {
    errors: [ { message: '必须年满18岁', field: 'age' } ],
    fields: {
      age: [
        { message: '必须年满18岁', field: 'age' }
      ]
    }
  }
  ```
