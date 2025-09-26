# CSS 常见面试题

## CSS 基础

## 概念

### 行框

在 CSS 视觉模型中，文本的布局遵循行盒模型（line box model）。行框是一个在块级格式化上下文（block formating context）中，用来包含同一行内的所有行内级盒子的矩形区域，换句话说浏览器在渲染一段文字的时候，每一行就是一个 line box。

```structured text
┌────────────────────────────┐ ← 块级容器（block box）
│┌──────────────────────────┐│
││   [line box 1]           ││ ← 第一行的行框
│└──────────────────────────┘│
│┌──────────────────────────┐│
││   [line box 2]           ││ ← 第二行的行框
│└──────────────────────────┘│
└────────────────────────────┘
```

先看一个比较简单的例子：

```html
<div class="container" style="text-align: center;">
  <img src="./favicon.ico" alt="">
</div>
```

这里的 img 就是一个行内级盒子。

再来看一个文字的例子：

```html
<p style="text-align: justify; background-color: #bfa;">这是一段文本，这段文本比较长，为了演示两端对齐的效果，需要让这段文字在这个固定宽度的段落中换行，从而观察 justify 的表现。这是一段文本，这段文本比较长，为了演示两端对齐的效果，需要让这段文字在这个固定宽度的段落中换行，从而观察 justify 的表现。这是一段文本，这段文本比较长，为了演示两端对齐的效果，需要让这段文字在这个固定宽度的段落中换行，从而观察 justify 的表现。</p>
```

因为这是一段连续的文本，所以只有一个行内级盒子，但是这一个行内级盒子会横跨多个行框。

### 同级元素

有两个 div 元素，第一个 div 元素的下外边距为 30px，第二个 div 元素的上外边距为 20px。他们两个的外边距就会发生折叠，他们两个真实的外边距就是两个的较大值，30px。

### 嵌套元素

当父元素的外边距和其第一个或最后一个子元素的外边距相遇时，如果父元素的外边距：

## 不知道什么属性

### line-height

## 分布属性

### text-align

在一个块级格式化上下文（block formating context）中，会包含一个或多个行框（line box），每个行框里面会依次排列行内级（inline box）盒子，比如文字，`<span>`，`<img>` 等。`text-align` 就是控制这些行内级盒子在行框中的水平分布。

一个最简单的例子：

```html
<div class="container" style="text-align: center;">
  <img src="./favicon.ico" alt="">
</div>
```

现象：图片会被水平居中。这里的行内元素就是 img，它会在其包含块（div 元素）内水平居中。

常用属性值：

* center：居中对齐
* justify：两端对齐，只有在多行文本的状态下能看出来效果，如果内容不够一行会调整字距
* left
* right

## 色彩空间

### sRGB

最通用。

### LCH

更接近人类视觉感知，更自然。

## CSS 自定义属性

CSS 自定义属性是 CSS3 的原生标准，允许用户在样式中自定义可复用的变量。

```css
:root {
  --dz-color-primary: #4093ff;
}

.dz-button {
  color: var(--dz-color-primary);
}
```

## 动画

## 奇怪机制

### 无能的 height

对于这段代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BoxSizing</title>
  <link rel="stylesheet" href="./reset.css">
  <style>
    #content {
      box-sizing: border-box;
      height: 0px;
      padding-bottom: 25px;
      border: 1px solid #ccc;
    }
  </style>
</head>
<body>
  <div id="content">
    <p>
      Lorem ipsum dolor sit amet consectetur, adipisicing elit. Hic corporis nesciunt dolorem possimus voluptatum minima sapiente ut a obcaecati facere? Est adipisci ducimus exercitationem numquam, alias itaque nulla deleniti similique?
    </p>
  </div>
</body>
</html>
```

最怪的一点就是明明设置了：

```css
#content {
  box-sizing: border-box;
  height: 0px;
  padding-bottom: 25px;
}
```

但是显示出来的样式为：border-bottom 距离 `#content` 的上边距还是存在 25px 的距离。

## SCSS/Sass
