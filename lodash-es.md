# Lodash-es

## omit

`omit` 函数是用来 **创建一个新的对象，排除掉指定的属性**。也就是说，使用 `omit` 可以移除对象中的某些属性，返回一个新的对象。

示例：

假设有一个对象，你想要去掉其中某些属性。

```javascript
import omit from 'lodash-es/omit';

const obj = { name: 'Alice', age: 25, job: 'Engineer' };

// 移除 'age' 和 'job' 属性
const newObj = omit(obj, ['age', 'job']);

console.log(newObj);  // { name: 'Alice' }
```

在这个例子中，`omit` 会返回一个新对象，其中 `age` 和 `job` 属性被去掉了，`name` 属性保留下来。
