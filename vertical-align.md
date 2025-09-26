# vertical-align

## 简介

`vertical-align` 控制行内元素（inline）或表格单元格（table-cell）在垂直方向上的对齐方式。

## 常见取值

常见取值及其含义：

* baseline: 元素的基线与父元素的基线对齐。
* top: 元素的顶部与所在行框的顶端对齐。
* middle: 元素的中线与父元素的基线 + x 的一半高度对齐。
* bottom: 元素的底端与所在行框的底端对齐。

用一段代码来对比四个取值：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>scroll-demo</title>
    <!-- <link rel="stylesheet" href="./reset.css" /> -->
    <style>
      span,
      img {
        border: 1px solid red;
      }
    </style>
  </head>
  <body>
    <p>baseline: <img src="./favicon.ico" width="5%" style="vertical-align: baseline" /></p>
    <p>middle: <img src="./favicon.ico" width="5%" style="vertical-align: middle" /></p>
    <p>top: <img src="./favicon.ico" width="5%" style="vertical-align: top" /></p>
    <p>bottom: <img src="./favicon.ico" width="5%" style="vertical-align: bottom" /></p>
  </body>
</html>
```

效果图：

![image-20250824163615324](https://kat-tc.oss-cn-chengdu.aliyuncs.com/undefinedimage-20250824163615324.png)
