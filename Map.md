# Map

映射，可迭代对象。顺序始终是 **插入顺序**。

## get/set

Map 对象的值的获取和设置必须使用 `get` 和 `set` 方法。直接使用 `[]` 的效果实际上是在对象上挂载属性。

`set(key, value)`

## keys/values/entries

特点：

* **实例方法**（直接在 Map 对象上调用，不是全局静态函数）。
* 返回的是**可迭代迭代器对象（IterableIterator）**，也就是说：它既是可迭代对象（能 for...of），也是迭代器（有 next() 方法）。这是 ES6 标准里特意设计的接口，称为 “迭代器也是迭代器本身的可迭代对象”。

例子：

```js
const map = new Map([["a", 1], ["b", 2]]);

console.log([...map.keys()]);   // ["a", "b"]
console.log([...map.values()]); // [1, 2]
console.log([...map.entries()]);// [["a", 1], ["b", 2]]

// 遍历方式
for (const [k, v] of map) {
  console.log(k, v);
}
```
