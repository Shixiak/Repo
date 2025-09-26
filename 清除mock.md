# 清除 mock

## mockClear

### 介绍

重置 mock 函数的调用记录。包括：

* `mock.calls`（每次调用的参数）
* `mock.instances`（调用时创建的实例）
* `mock.contexts`（调用时的 `this` 值）
* `mock.results`（返回值）

它不会：

* 删除 `mock` 本身
* 删除通过 `mockImplementation` / `mockReturnValue` 等设置的行为

所以它只是清空调用痕迹，不影响函数的实现。

### 使用场景

常见在多个测试之间或测试步骤之间清空调用记录，确保测试互不干扰。

```ts
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
