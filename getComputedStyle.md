# getComputedStyle

## 简介

`getComputedStyle` 是浏览器提供的 DOM API，主要作用是 获取元素的最终计算样式。它返回的是元素在页面上真正生效的样式值，而不仅仅是你在 CSS 文件或行内样式里写的值。

---

## 基本用法

```js
const element = document.querySelector('.box');
const style = getComputedStyle(element);
console.log(style.width);   // 返回计算后的宽度
console.log(style.color);   // 返回实际应用的颜色
```

## 特点

返回的是最终计算值，例如 `width: auto;` 在 CSS 里是自动，但在 `getComputedStyle` 里会变成像素值（如 `"500px"`）。百分比、继承值、关键字（如 `inherit`、`initial`）都会被解析成浏览器实际渲染时的值。
