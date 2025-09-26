# this 绑定函数

## 简介

call、apply、bind 三个方法，它们都是 `Function.prototype` 上的方法，用来显式指定函数执行时的 this，并控制参数传递方式。

## call

语法：

```js
func.call(thisArg, arg1, arg2, ...);
```

立即调用函数，this 被绑定为 `thisArg`，剩余参数逐个传入。

## apply

语法：

```js
func.apply(thisArg, [argsArray]);
```

立即调用函数，`this` 绑定为 `thisArg`。

参数形式：一个数组（或类数组对象）。

## bind

```js
const bound = func.bind(thisArg, arg1, arg2, ...);
bound(...moreArgs);
```

不会立即执行函数，而是返回一个新函数，this 永远绑定为 thisArg，并且可以预先传入部分参数（柯里化）。

参数形式：跟 call 类似，逗号分隔。
