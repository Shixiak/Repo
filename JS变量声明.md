# 变量声明

## 简介

在 JavaScript 中，var、let 和 const 都可以用来声明变量，但它们在作用域、提升（hoisting）、可重复声明、以及值的可变性等方面存在明显差异。

## 作用域

作用域方面的区别:

* var
  * 具有函数作用域（function scope）。
  * 如果在函数内部用 var 声明变量，则在整个函数内部都能访问；如果在函数外声明，则是全局变量。
* let 和 const
  * 具有块级作用域（block scope）。
  * 在 `{ ... }` 内声明的变量只在该代码块中有效，出了这个块就无法访问。

## 变量提升

变量提升（Hoisting）方面的区别：

* var
  * 存在变量提升：声明会被提升到作用域顶部，但赋值不会。
  * 在声明之前访问，值为 `undefined`。
* let 和 const
  * 也会提升，但会处于“暂时性死区（Temporal Dead Zone，TDZ）”，在实际声明之前访问会抛出 `ReferenceError`。

## 重复声明

重复声明方面的区别：

* var: 同一作用域内允许重复声明，后一次声明会覆盖前一次声明。
* let 和 const: 同一作用域内不允许重复声明，否则会报错。

## 值的可变性

声明和赋值：

* var 和 let 都可以在声明后再赋值和修改。
* const: 必须在声明时赋值，之后不能再重新赋值。

## 全局对象属性

是否挂载到全局对象：

* var: 在全局作用域中声明的 var 变量会挂载到 window（浏览器环境）或 global（Node.js 环境）对象上。
* let 和 const: 在全局作用域中声明的变量不会挂载到全局对象上，更安全。

## 示例

for 循环中的 let 和 var

```js
const funcs = [];
for (let i = 0; i < 3; i++) {
  funcs.push(() => console.log(i));
}

funcs[0](); // 0
funcs[1](); // 1
funcs[2](); // 2
```

let 和 var 在 for 循环中的不同表现：

* var i：只有一个绑定（函数/全局级）。所有迭代中的箭头函数都指向这一个绑定；循环结束后 i === 3，所以都打印 3。
* let i（在 for 头部）：规范中（CreatePerIterationEnvironment）会在每次迭代克隆一个新的环境，并为 i 创建新的绑定，其初始值来自上一次迭代更新后的值。因此三个函数分别指向三个不同的绑定，依次打印 0、1、2。
