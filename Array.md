# Array

数组。

数组的构造方法：

创建一个长度为 n 的数组。

```js
Array.from({length: n})
```

创建一个长度为 n 的数组，数组的每一个元素都是 `[]`。

```js
Array.from({length: n}, () => []);
```

数组有一些常用方法：

`sort(cmp)`：默认按照字典序排序。`(a, b) => a - b` 升序，`(a, b) => b - a` 降序。

## sort

就地排序，返回同一个数组的引用。

默认排序规则：

* 将元素先转为**字符串**，再按 **UTF-16 码点**做字典序比较（**不是**数字大小）。

  ```js
  [2, 10, 1].sort(); // ["1", "10", "2"] → [1, 10, 2]
  ```

* `undefined` 与“空槽”（稀疏数组里的缺口）会被放到末尾；空槽仍保持为空槽。

自定义比较函数 `comparefn(a, b)`，返回值语义：

* `< 0`：`a` 排在 `b` 前
* `= 0`：顺序不变（稳定）
* `> 0`：`a` 排在 `b` 后

常见写法：

```js
// 数字升序 / 降序
nums.sort((a, b) => a - b);
nums.sort((a, b) => b - a);
```

## reduce

`Array.prototype.reduce()` 是 JavaScript 中用于**将数组中的所有元素按照指定的规则，归约（reduce）成单个值**的方法。

基本语法：

```js
array.reduce(callback[, initialValue])
```

以一个求和例子为例：

```js
const arr = [1, 2, 3, 4];
const sum = arr.reduce((acc, cur) => acc + cur, 0);
console.log(sum); // 10
```

## some

some 用于测试数组中是否至少有一个元素满足给定的条件。

语法：

```js
arr.some(callback(element, index, array), thisArg?)
```

参数：

* **callback**：测试函数，对数组的每个元素调用。
  * `element` → 当前元素
  * `index` → 当前索引
  * `array` → 原数组
* **thisArg**（可选）：执行回调时的 `this` 值。

返回值：

* 如果有一个元素使 `callback` 返回 `true`，`some` 就会返回 **`true`**。
* 如果所有元素都不满足条件，则返回 **`false`**。

基础用法

```js
const arr = [1, 2, 3, 4, 5];

const hasEven = arr.some(n => n % 2 === 0);
console.log(hasEven); // true，因为 2、4 是偶数

const biggerThanTen = arr.some(n => n > 10);
console.log(biggerThanTen); // false，因为没有元素大于 10
```

## 修改 length

length 是数组的特殊属性，它不是只读的，你可以手动修改。

当你把 length 设置为一个更小的值时，数组会被截断（truncate）。

## 空槽

在 JavaScript 中：

```js
let arr = [];
arr[2] = 5;
console.log(arr);       // [ <2 empty items>, 5 ]
console.log(arr.length) // 3
```

### 原理

* **数组是对象**：JS 数组的索引本质上是对象的键（字符串形式的非负整数）。
* **跳着赋值**：当你直接给一个大于当前 `length` 的索引赋值时，JS 会：

  1. 自动把 `length` 扩展到 `index + 1`。
  2. 中间的元素不会自动填充 `undefined`，而是 **稀疏数组**（Sparse Array）中的“空槽”（empty item）。
* **空槽和 `undefined` 不一样**：

  * `arr[1]` 在这里是一个空槽，`1 in arr` 为 `false`。
  * `arr[1] === undefined` 结果是 `true`，但那是因为读取一个不存在的属性会返回 `undefined`。

以下代码可以验证：

```js
console.log(arr[1]);        // undefined
console.log(1 in arr);      // false
console.log(arr.hasOwnProperty(1)); // false
```

### 注意事项

* 这种稀疏数组会让 `for...in`、`forEach`、`map` 等迭代行为表现不同：

  ```js
  arr.forEach(v => console.log(v)); // 只会打印 5，不会迭代空槽
  ```
