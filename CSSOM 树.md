# CSSOM 树

## 简介

CSSOM(CSS Object Model): 是浏览器在解析 CSS 样式表 后，构建的一棵面向对象的树形结构。

## 示例

假设有这样的 HTML 和 CSS：

```html
<body>
  <h1>Hello</h1>
  <p class="note">World</p>
</body>
```

```css
h1 {
  color: red;
}
p.note {
  font-size: 14px;
  color: blue;
}
```

浏览器会生成大致如下的 CSSOM 树：

```text
CSSOM
└── CSSStyleSheet
    ├── CSSStyleRule (selector: "h1")
    │      └── { color: red }
    └── CSSStyleRule (selector: "p.note")
           └── { font-size: 14px; color: blue }
```
