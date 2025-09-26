# Promise

## 回调性质

Promise 的回调会被放入微任务队列。

## Promise.resolve

`Promise.resolve` 是一个便捷方法，通常用于“把一个值包成 Promise”。

示例

```js
Promise.resolve(1)
```

* 这是一个 **静态方法**，直接返回一个 **已经处于 fulfilled 状态**、值为 `1` 的 Promise。
* 它几乎是“立即完成”的，不会进入 `executor` 函数。
* 等价于：

  ```js
  new Promise((resolve) => resolve(1))
  ```

  （但效率更高，不需要额外创建执行器函数）。

## 链式 Promise

以下面的代码为例：

```js
Promise.resolve(1)
  .then(res => {
    console.log("第一个 then:", res)
    return new Promise(resolve => {
      setTimeout(() => resolve("延迟的结果"), 1000)
    })
  })
  .then(res => {
    console.log("第二个 then:", res)
  })
```

## 手写 Promise

### 支持多次调用以及链式调用 then

实现 then 函数的全部功能，以及 catch 和 finally 函数。但没有实现 resolve, reject 等静态方法。

```js
// —— 可直接整体替换（仅使用 queueMicrotask） ——
function resolvePromise(promise2, x, resolve, reject) {
  if (promise2 === x) return reject(new TypeError('Chaining cycle detected'));

  if (x !== null && (typeof x === 'object' || typeof x === 'function')) {
    let called = false;
    try {
      const then = x.then; // 可能是 getter，会抛错
      if (typeof then === 'function') {
        then.call(
          x,
          (y) => {
            if (called) return; called = true;
            resolvePromise(promise2, y, resolve, reject);
          },
          (r) => {
            if (called) return; called = true;
            reject(r);
          }
        );
      } else {
        resolve(x);
      }
    } catch (e) {
      if (called) return;
      called = true;
      reject(e);
    }
  } else {
    resolve(x);
  }
}

class MyPromise {
  static PENDING   = 'pending';
  static FULFILLED = 'fulfilled';
  static REJECTED  = 'rejected';

  constructor(executor) {
    this._state = MyPromise.PENDING;
    this._value = undefined;
    this._fulfillCbs = [];
    this._rejectCbs = [];

    const resolve = (val) => {
      if (this._state !== MyPromise.PENDING) return;

      // 采用外部 MyPromise（可选，但更贴近原生）
      if (val instanceof MyPromise) {
        return val.then(resolve, reject);
      }

      this._state = MyPromise.FULFILLED;
      this._value = val;
      queueMicrotask(() => {
        this._fulfillCbs.forEach(fn => fn());
        this._fulfillCbs.length = 0;
      });
    };

    const reject = (reason) => {
      if (this._state !== MyPromise.PENDING) return;
      this._state = MyPromise.REJECTED;
      this._value = reason;
      queueMicrotask(() => {
        this._rejectCbs.forEach(fn => fn());
        this._rejectCbs.length = 0;
      });
    };

    try {
      executor(resolve, reject);
    } catch (e) {
      reject(e);
    }
  }

  then(onFulfilled, onRejected) {
    const onFul = typeof onFulfilled === 'function' ? onFulfilled : v => v;
    const onRej = typeof onRejected  === 'function' ? onRejected  : e => { throw e; };

    const promise2 = new MyPromise((resolve, reject) => {
      const runFulfilled = () => {
        try {
          const x = onFul(this._value);
          resolvePromise(promise2, x, resolve, reject);
        } catch (e) { reject(e); }
      };
      const runRejected = () => {
        try {
          const x = onRej(this._value);
          resolvePromise(promise2, x, resolve, reject);
        } catch (e) { reject(e); }
      };

      if (this._state === MyPromise.FULFILLED) {
        queueMicrotask(runFulfilled);
      } else if (this._state === MyPromise.REJECTED) {
        queueMicrotask(runRejected);
      } else {
        this._fulfillCbs.push(() => queueMicrotask(runFulfilled));
        this._rejectCbs.push(() => queueMicrotask(runRejected));
      }
    });

    return promise2;
  }
}
```

### 学习样例

下面的案例会提到一个概念：普通值，即为非 MyPromise 的返回值。

可以通过以下的一些案例逐步学习：

* 执行器函数里面是同步调用 resolve，注册多次 then，返回不同的普通值。
* 执行器里面写异步代码，只写一个 then，且不返回结果。
* 执行器里面写异步代码，写多个 then，均不返回结果。
