# color-mix

## 简介

`color-mix()` 是 **CSS Color Module Level 5** 提供的一个函数，用于在 CSS 中直接混合两种颜色。它的作用相当于在 SCSS / Less 中写颜色函数，但这是 **浏览器原生支持的 CSS 函数**。

语法如下：

```css
color-mix(in <color-space>, <color1> <percentage>?, <color2> <percentage>?)
```

* `in <color-space>`：指定混合的颜色空间，目前主流浏览器支持 `srgb`、`srgb-linear`、`display-p3` 等。最常用的是 `srgb`。
* `<color1> <percentage>?`：第一种颜色，可以选择性附带百分比。
* `<color2> <percentage>?`：第二种颜色，同样可以选择性附带百分比。

如果百分比省略，默认是平均分配，或者按剩余百分比分配。

## 使用示例

### 示例 1：平均混合两种颜色

```css
.demo1 {
  color: color-mix(in srgb, red, blue);
}
```

50% 红色 + 50% 蓝色，得到紫色。

### 示例 2：指定比例

```css
.demo2 {
  color: color-mix(in srgb, red 30%, blue 70%);
}
```

30% 红色 + 70% 蓝色，颜色偏蓝。

---

### 示例 3：和透明度混合（实现淡化效果）

```css
.demo3 {
  background-color: color-mix(in srgb, red 40%, transparent);
}
```

40% 红色 + 60% 白色背景透出

---

### 示例 4：和黑白混合（常用于生成浅色/深色）

```css
.light {
  background-color: color-mix(in srgb, blue 30%, white);
}

.dark {
  background-color: color-mix(in srgb, blue 80%, black);
}
```
