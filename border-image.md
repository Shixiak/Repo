# border-image

## 基本工作机制

把一张图片按上/右/下/左四个内切线切成 9 块：4 角、4 条边、1 个中心。

绘制时：

* 四个角**不缩放**，直接贴在元素四角（受 `border-image-repeat` 影响很小）。
* 四条边按水平方向/垂直方向**拉伸或平铺**以匹配边框长度。
* 中心区域是否被填充由 `fill` 决定（见 `border-image-slice`）。

## border-image-source

特性：

* **作用**：指定边框图像源。
* **语法**：`border-image-source: <image> | none;`
* **取值**：`url(...)`、`linear-gradient(...)` / `radial-gradient(...)`（现代浏览器支持）、或 `none`。
* **默认值**：`none`（即不使用图像边框）。

```css
.box { border-image-source: url(frame.png); }
```

## border-image-slice

* **作用**：从图像四边**向内切割**的距离（决定九宫格的切线位置），还可控制是否填充中心。
* **语法**：`border-image-slice: [<number>|<percentage>]{1,4} && fill?;`
  * 1\~4 个值：分别对应 **上 右 下 左**；省略按 CSS 2D 盒模型速记规则补全（上|左右|下）。
  * `<number>` 以**像素**（图像内在像素）解释；`<percentage>` 相对**源图像尺寸**。
  * 可附加 `fill` 关键字：用中心片填充元素背景（在内容区域下方）。
* **默认值**：`100%`（无 `fill`）。
* **要点**：
  * `slice` 仅决定**切线位置**，不等于最终边框厚度（厚度由 `border-image-width` 决定）。

```css
/* 从四边各切 30px，并填充中心 */
.box { border-image-slice: 30 fill; }

/* 上下 20%，左右 30%，不填充中心 */
.box { border-image-slice: 20% 30%; }
```

## border-image-repeat

border-image-repeat:

* **作用**：控制四条边上的**平铺/缩放策略**（角块始终单元贴合）。
* **语法**：`border-image-repeat: [stretch|repeat|round|space]{1,2};`

  * 单值：四边统一。
  * 双值：前者用于**水平边**（上/下），后者用于**垂直边**（左/右）。
* **含义**：

  * `stretch`：**拉伸**一块到边长度（默认）。
  * `repeat`：按原尺寸**平铺**，末端可能裁切。
  * `round`：按整数个**平铺**，最后**微调每块尺寸**以整除边长度（无裁切）。
  * `space`：**不缩放**，用间隙把整段对齐（两端留空隙，块间均匀分布）。

```css
.box { border-image-repeat: round; }        /* 常用于避免边长不整除导致的裁切 */
.box { border-image-repeat: repeat space; } /* 水平 repeat，垂直以 space 对齐 */
```
