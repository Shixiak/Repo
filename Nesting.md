# 嵌套

现在主流浏览器已原生支持 CSS Nesting。

这段：

```css
.parent {
  .child { ... }
}
```

在无任何 PostCSS 嵌套插件的情况下也能直接生效，等价于：

```css
.parent .child { ... }
```

可省略 `&` 的情况

当内层选择器以 **类 `.`、ID `#`、类型选择器、伪类/伪元素 `:`、或组合符** 开头时，浏览器能明确这是“嵌套的选择器”，而不是属性名，于是**允许省略 `&`**：

```css
.parent { 
  .child { color: red; } 
}
/* => .parent .child { ... } */

.parent {
  p { margin: 0; } 
}
/* => .parent p { ... } */

.parent {
  :focus-within { ... }
}
/* => .parent :focus-within { ... } */

.parent {
  > .child { ... } 
}
/* => .parent > .child { ... } */
```

必须写 `&` 的情况（不写会跑偏或无效）

**要把伪类/类“附着”在父选择器上**时：

```css
.btn {
  /* 错误含义：.btn :hover（后代的 hover） */
  :hover { ... }

  /* 正确：.btn:hover */
  &:hover { ... }
}
```
