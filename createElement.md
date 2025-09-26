# createElement

## 简介

创建一个元素，但并不插入文档。

## 示例

```js
const container = document.createElement('div');
```

这行代码只是在内存中创建了一个新的 `<div>` 元素节点，并把它赋值给变量 container。此时它还没有被插入到 document 的 DOM 树中。

## 使用 document 调用的意义

### document 是什么

在浏览器里，全局的 `document` 对象代表着 当前 HTML 文档的入口点。它不仅保存了 DOM 树，还提供了很多方法来操作这棵树。

* `document` 相当于是 DOM 的“根”，它知道自己管理的上下文。
* 所有的节点（元素、文本、注释等）必须“属于”某个文档。

### 为什么是 document.createElement

当你调用：

```js
const div = document.createElement('div');
```

实际上发生的是：

* `document` 创建了一个 归属于它自己的节点。
* 这个新节点会记录它是属于哪个 `document` 的（`node.ownerDocument` 指针）。
* 如果未来把这个节点插入到页面上，它才能正确工作，和当前 DOM 树保持一致。

换句话说，调用 `document.createElement` 是为了保证元素和正确的文档上下文绑定。

### 如果不是 document.createElement 会怎样？

在浏览器里，标准规定你必须通过某个 `document` 来创建元素。

* 这样能避免跨文档（iframe、多窗口）混乱。
* 例如在 iframe 里，如果你用外部的 `document` 来创建元素，再插进去，就可能产生样式或行为不一致的问题。

所以，`createElement` 放在 `document` 上并不是偶然，而是 语义上强调“元素属于文档”。
