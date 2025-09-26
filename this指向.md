# this 指向问题

## 简介

this 的指向不是在函数定义时决定的，而是函数调用时决定的。

## this 的绑定规则

### 默认绑定

分为严格模式和非严格模式：

* 在普通函数调用中，若没有任何修饰，this 指向 全局对象（浏览器里是 window，Node.js 中是 global）。
* 严格模式下，默认绑定的 this 为 undefined。

示例代码：

```js
function foo() {
  console.log(this);
}

// 非严格模式：window / global
// 严格模式：undefined
foo(); 
```

### 隐式绑定

当函数作为某个对象的方法被调用时，this 指向该对象。

```js
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name);
  }
};
obj.greet(); // Alice
```

### 显式绑定

显示指定函数内部的 `this`。

参考：[this 绑定](./this绑定.md)

### new 绑定

当函数通过 new 调用时，会创建一个新的对象，并把 this 绑定到这个新对象上。

```js
function Person(name) {
  this.name = name;
}
const p = new Person("Charlie");
console.log(p.name); // Charlie
```

### 箭头函数

箭头函数不会创建自己的 this，而是继承定义时所在作用域的 this。

```js
const obj = {
  name: "Dana",
  say: () => {
    console.log(this.name);
  }
};
obj.say(); // undefined（this 来自外层：全局/undefined）
```

配合 setTimeout 或回调时特别有用：

```js
function Person(name) {
  this.name = name;
  setTimeout(() => {
    console.log(this.name); // 正确捕获 this
  }, 1000);
}
new Person("Eve"); // Eve
```

## 复杂案例分析

### 回调

obj.greet 被直接赋值给 callback，setTimeout 会直接使用 `callback()` 的形式调用函数。

```js
const obj = {
  name: "Foo",
  greet() {
    console.log(this.name);
  }
};

setTimeout(obj.greet, 1000);
// this 丢失 → window/undefined
```

### 构造函数和普通函数

构造函数若返回一个对象，则 this 会被该对象替代。

```js
function Foo() {
  this.value = 42;
  return { value: 100 }; 
}

const f = new Foo();
console.log(f.value); // 100
```

### 箭头函数和普通函数

箭头函数不绑定自己的 `this`，所以 `obj.arrow` 的 `this` 并不是 `obj`。

```js
const obj = {
  value: 10,
  normal() {
    console.log(this.value);
  },
  arrow: () => {
    console.log(this.value);
  }
};

obj.normal(); // 10
obj.arrow();  // undefined
```

## 参考资料

[箭头函数](./箭头函数.md)
