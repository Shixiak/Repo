# vi

在 Vitest 中，`vi` 是 Vitest 提供的全局对象（类似于 Jest 里面的 `jest`），用于测试过程中的模拟（mock），间谍（spy），定时器控制等辅助功能。

它的作用可以分成三大类：

## Mock 函数与间谍函数

`vi` 提供了生成和操作 mock 函数的 API，用于代替真实函数并记录调用情况。

```js
import { vi } from 'vitest';

test('mockClear 示例', () => {
  const fn = vi.fn().mockReturnValue('hello');

  fn('a');
  fn('b');

  console.log(fn.mock.calls.length); // 2
  console.log(fn.mock.calls); // [['a'], ['b']]

  // 清除调用记录
  fn.mockClear();

  console.log(fn.mock.calls.length); // 0
  console.log(fn.mock.calls); // []
  
  // 行为还在
  console.log(fn()); // 'hello'
});
```

## Mock 模块

在 Vitest 中，`vi.mock()` 是模块级别的模拟（mock）方法，用于在测试中替换整个模块的导出，从而控制依赖的行为，避免执行真实的逻辑。

### 基本语法

mokc 的语法如下：

```ts
vi.mock(modulePath, factory?, options?)
```

参数解读：

* `modulePath`：要模拟的模块的路径（相对于当前文件，或 npm 包名）。
* `factory`：返回一个对象的函数，返回的对象用于替代原模块的导出内容。
* `options`：一些高级控制选项，比如 `virtual: true`（声明一个并不存在的虚拟模块）。

### 工作原理

Vitest 在加载被测文件之前，会**拦截 `import` 或 `require`** 对目标模块的加载，并用你在 `vi.mock()` 中提供的对象替代真实模块的导出。

* 如果 **不传 `factory`**，Vitest 会根据模块类型自动创建一个**全自动模拟版本**（Auto Mock），即所有导出都是可追踪的 `vi.fn()`。
* 如果 **传 `factory`**，你可以自己定义具体的替代实现。
* `vi.mock()` 必须在**模块的顶层调用**（不能放在函数里面），这样 Vitest 才能在模块解析前完成拦截。

### 常见用法

#### 自动模拟整个模块

```ts
// math.ts
export function add(a: number, b: number) {
  return a + b;
}
export function mul(a: number, b: number) {
  return a * b;
}

// math.test.ts
import { add, mul } from './math';

vi.mock('./math'); // 不提供 factory，自动 mock

test('mocked math', () => {
  add.mockReturnValue(10); // 自动生成的 vi.fn()
  expect(add(1, 2)).toBe(10);
  expect(mul()).toBeUndefined(); // 默认没有返回值
});
```

#### 自定义替代实现

```ts
vi.mock('./math', () => {
  return {
    add: vi.fn(() => 42),
    mul: vi.fn(() => 99)
  }
});

import { add, mul } from './math';

test('custom mock', () => {
  expect(add()).toBe(42);
  expect(mul()).toBe(99);
});
```

#### 模拟第三方库

```ts
vi.mock('axios', () => {
  return {
    default: {
      get: vi.fn(() => Promise.resolve({ data: { name: 'MockUser' } }))
    }
  }
});

import axios from 'axios';

test('mock axios get', async () => {
  const res = await axios.get('/user');
  expect(res.data.name).toBe('MockUser');
});
```

#### 虚拟模块（测试中构造一个不存在的模块）

```ts
vi.mock('my-virtual-module', () => {
  return { foo: vi.fn(() => 'bar') };
}, { virtual: true });

import { foo } from 'my-virtual-module';

test('virtual module', () => {
  expect(foo()).toBe('bar');
});
```

## spyOn

`vi.spyOn()`和 `vi.mock()` 类似，都是 Vitest 提供的测试辅助 API，但是用途不同——`spyOn` 更像是“监听器 + 可选替换实现”，而不是整个模块的替换。

### 基本作用

`vi.spyOn()` 用于**监视对象上的方法调用**，记录调用次数、参数、返回值等信息，并且**可以选择性地替换该方法的实现**。

* **默认情况下**：方法的原始实现仍会执行（只是多了一个“监听”功能）。
* **如果你需要模拟行为**：可以用 `.mockImplementation()` 或 `.mockReturnValue()` 改变它的实现。

### 语法

```ts
vi.spyOn(object, methodName)
vi.spyOn(object, methodName, accessType)
```

* **`object`**：包含要监听方法的对象
* **`methodName`**：字符串，方法名
* **`accessType`** *(可选)*：`'get'` / `'set'`，用于监听 getter 或 setter 属性

### 基础示例

```ts
const user = {
  getName() {
    return 'Alice';
  }
};

test('spy on method', () => {
  const spy = vi.spyOn(user, 'getName');

  const name = user.getName(); // 真实方法仍然执行

  expect(spy).toHaveBeenCalledTimes(1);
  expect(spy).toHaveReturnedWith('Alice');
});
```

这里 `spy` 是一个 **mock 函数**，你可以用所有 `expect(...).toHave...` 这类断言检查它的调用情况。

### 修改实现（部分替换）

```ts
const user = {
  getName() {
    return 'Alice';
  }
};

test('spy and mock implementation', () => {
  const spy = vi.spyOn(user, 'getName').mockImplementation(() => 'Bob');

  expect(user.getName()).toBe('Bob'); // 新实现生效
  expect(spy).toHaveBeenCalledTimes(1);
});
```

这样可以在不改动真实代码的情况下，模拟不同返回值或行为。

### 监听 getter / setter

```ts
const obj = {
  _value: 1,
  get value() {
    return this._value;
  },
  set value(v) {
    this._value = v;
  }
};

test('spy on getter and setter', () => {
  const getSpy = vi.spyOn(obj, 'value', 'get');
  const setSpy = vi.spyOn(obj, 'value', 'set');

  obj.value = 10;
  expect(setSpy).toHaveBeenCalledWith(10);

  const val = obj.value;
  expect(getSpy).toHaveReturnedWith(10);
});
```

### 恢复原实现

* **`mockRestore()`**：恢复到方法的原始实现
* **`vi.restoreAllMocks()`**：恢复所有用 `spyOn` 创建的监听

```ts
test('restore original', () => {
  const spy = vi.spyOn(user, 'getName').mockReturnValue('Charlie');
  expect(user.getName()).toBe('Charlie');

  spy.mockRestore(); // 恢复
  expect(user.getName()).toBe('Alice');
});
```

### 与 vi.mock 的区别

| 特性      | `vi.spyOn()` | `vi.mock()`         |
| ------- | ------------ | ------------------- |
| 作用范围    | 单个对象方法       | 整个模块                |
| 是否保留原实现 | 默认保留         | 默认替换为 `vi.fn()`     |
| 调用位置要求  | 运行时调用即可      | 必须在模块顶层调用（import 前） |
| 使用场景    | 监控或临时替换某个函数  | 全部依赖替换，隔离外部模块       |
