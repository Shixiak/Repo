# debounce

## 简介

防抖，只有在最后一次触发事件后，等待一段时间（延迟时间）再执行函数，如果在等待时间内又触发了事件，就重新计时。

## 手写实现

### 基础版

```js
function debounce(fn, delay) {
  let timer = null;
  return function (...args) {
    if (timer) clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

### 标准版

包含 leading 和 trailing 两种配置。

```js
function debounce(fn, wait, options = {}) {
  const leading = options.leading || false;
  const trailing = options.trailing !== false; // 默认 true

  let timer = null;
  let lastArgs = null;
  let lastThis = null;

  return function (...args) {
    const isIdle = (timer === null);
    lastArgs = args;
    lastThis = this;  TODO 暂时没有看懂这一步的意义

    if (isIdle && leading) {
      fn.apply(lastThis, lastArgs);
      lastArgs = lastThis = null;
    }

    if (timer) clearTimeout(timer);

    if (trailing) {
      timer = setTimeout(() => {
        timer = null;
        if (lastArgs) {
          fn.apply(lastThis, lastArgs);
          lastArgs = lastThis = null;
        }
      }, wait);
    } else if (isIdle) {
      // 仅 leading 模式：设冷却
      timer = setTimeout(() => { timer = null; }, wait);
    }
  };
}
```

## cancel/flush

在 **lodash**（以及早期的 underscore 1.9+）中，`_.debounce` 返回的函数对象附带了两个额外方法：

* `debounced.cancel()`：立即取消已延迟的调用。
* `debounced.flush()`：立即执行当前等待中的调用。
