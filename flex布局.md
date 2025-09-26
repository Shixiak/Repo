# flex 布局

## 基本概念

Flex 布局是一种一维布局方式，主要用于在一行或一列上对元素进行高效、灵活的对齐和分配空间。

容器：设定 `display: flex;` 的元素。

项目：容器内部的子元素，自动成为 flex item。

## 两条轴

主轴（main axis）：默认是 水平方向（从左到右）。

交叉轴（cross axis）：与主轴垂直，默认是 垂直方向（从上到下）。

通过 `flex-direction` 可以改变主轴方向。

## 容器属性

### flex-direction（主轴方向）

row（默认）：主轴水平，从左到右。

row-reverse：主轴水平，从右到左。

column：主轴垂直，从上到下。

column-reverse：主轴垂直，从下到上。

## justify-content（主轴对齐）

flex-start：起点对齐（默认）。

flex-end：终点对齐。

center：居中。

space-between：两端对齐，中间均匀分布。

space-around：每个项目两侧间隔相等。

space-evenly：项目间隔完全平均。

## align-items（交叉轴对齐）

stretch（默认）：如果 item 没有设置高度，拉伸填满容器。

flex-start：顶部对齐。

flex-end：底部对齐。

center：居中对齐。

baseline：以文字基线对齐。

## flex-wrap（换行方式）

nowrap（默认）：不换行。

wrap：自动换行。

wrap-reverse：换行但方向反转。

## flex-flow（简写属性）

flex-direction + flex-wrap，如：

```css
flex-flow: row wrap;
```

## 项目属性（作用于子元素）

### order

定义项目的排列顺序，数值越小越靠前。

### flex-grow

定义项目的放大比例，默认为 0（不放大）。

### flex-shrink

定义项目的缩小比例，默认为 1（空间不足时会缩小）。

### flex-basis

定义项目在分配空间前的初始大小。

### flex（简写）

flex-grow flex-shrink flex-basis 的缩写。

常见用法：

* flex: 1; → 占据剩余空间。
* flex: none; → 不放大不缩小，按内容大小。

### align-self

允许单个项目在交叉轴方向上覆盖 align-items 的设置。
