# DOM 树

## 概念

DOM 树（Document Object Model Tree）是浏览器在加载和解析 HTML 文档时，依据文档的层级结构生成的一种树形数据结构。

## 示例

html 结构

```html
<div>
  <h1>Hello, World!</h1>
  <p>This is a paragraph.</p>
</div>
```

对应的 DOM 树结构

```text
div
├── h1
│   └── "Hello world!"
└── p
    └── "This is a paragraph"
```
