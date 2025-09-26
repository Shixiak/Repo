# getBoundingClientRect

## 简介

`getBoundingClientRect()` 用于获取元素相对于 视口 (viewport) 的位置和尺寸信息。
它返回的是一个 `DOMRect` 对象，里面包含元素的大小（宽高）和边界坐标（top, left, right, bottom）。

---

## DOMRect

调用结果类似这样：

```js
const rect = element.getBoundingClientRect();
console.log(rect);
```

`rect` 是一个 `DOMRect` 对象，常见属性有：

* 位置

  * `rect.x` / `rect.left`：元素左边界到视口左边的距离
  * `rect.y` / `rect.top`：元素上边界到视口顶边的距离
  * `rect.right`：元素右边界到视口左边的距离
  * `rect.bottom`：元素下边界到视口顶边的距离

* 尺寸

  * `rect.width`：元素的宽度（包含 padding + border，不包含 margin）
  * `rect.height`：元素的高度（包含 padding + border，不包含 margin）

注意：这些数值是 浮点数，可能带小数，因为浏览器渲染时可能存在亚像素。
