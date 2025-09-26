# DocumentFragment

## 简介

`DocumentFragment` 是 DOM 标准定义的一种 Node 类型。

特点：

1. 它是一个最小化文档对象，但不是 Document，也不属于 DOM 树。
2. 它可以像元素一样包含子节点（Element、Text 等），支持 `appendChild`、`querySelector`、`innerHTML` 等常见 DOM API。
3. 当将 `DocumentFragment` 插入到文档时，它本身不会被插入，只有其中的子节点会被迁移到目标位置。插入后，`DocumentFragment` 会变为空。

## 基本用法

```js
const frag = document.createDocumentFragment(); // 创建 DocumentFragment

// 在其中添加多个节点
for (let i = 0; i < 3; i++) {
  const li = document.createElement("li");
  li.textContent = `item ${i}`;
  frag.appendChild(li);
}

// 一次性插入到目标节点
document.querySelector("#list").appendChild(frag);

// 注意：#list 内会多出 3 个 <li>，而 frag 已变为空。
```

## 性能优化作用

使用 DocumentFragment 可以优化性能：

* 减少 DOM 更新次数：如果直接向目标容器逐个 appendChild，浏览器会多次更新渲染树。用 DocumentFragment 先批量构建，再整体插入，只触发一次更新。
* 避免不必要的布局计算：DocumentFragment 不在渲染树中，因此在向其中插入节点时不会触发布局与重绘。
