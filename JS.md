# JS 篇

## JS 基础

### 数据类型概述

#### 基本数据类型（Primitive Types）

基本数据类型：

* Number
* Boolean
* Undefine
* String
* Symbol
* BigInt
* Null

### undefined 和 null 的区别

一个变量如果被声明了但是没有初始化，那么它的值就是 undefined。但是如果要明确一个变量当前确实没有值，我们就应该给他赋值为 null。

两者在运算上也有一些差异，Number(undefined) 的结果是 NaN，Number(null) 的结果是 0.

另外 null == undefined 为 true，但是 null === undefined 为 false。

## 内置对象

### Number

作为函数调用：

```js
Number("123")  // 123
```

作为构造函数：

```js
new Number(123);
```

返回的是一个包装对象。

### String

String 用于表示一些列字符的有序集合，每个字符由一个或多个 16 位 UTF-16 代码单元组成。

两种存在形式：

字符串字面量（Primitive String）

```js
let str1 = "hello";
let str2 = "world";
let str3 = "Hello world";
```

String 对象

通过 `new String(...)` 创建的对象形式。

```js
let objStr = new String('Hello');
console.log(typeof objStr);
```

JS 会在必要时（访问属性，调用方法）自动装箱，把基本字符串包装成 `String` 对象。

String 对象有一些常用的方法

### Math

#### 取整相关方法

##### Math.floor

向下取整。

```js
Math.floor(3.5);  // 3
```

##### Math.cell

向上取整。

```js
Math.cell(3.5); // 4
```

### 字符串转为整数的方法

#### parseInt

语法：

```ts
parseInt(string, radix);
```

#### Unary +

语法：

```ts
+"123"
```

## ES6 新特性

### Proxy 可以实现什么功能

Proxy 是 JS 在 ES6 引入的一个内嵌对象，用于创建一个对象的代理。我们可以通过这个代理对象拦截和自定义对目标对象的各种操作。比如读取属性，修改属性，删除属性，判断属性是否存在等。
