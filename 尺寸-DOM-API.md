# 尺寸 DOM API

## scroll 家族

### scrollWidth

scrollWidth 的含义大致是可滚动内容的宽度。

如果情况足够理想，它的表达式是：scrollWidth = box.padding + content.margin + content 可见框宽度。

但是 scrollWidth 的取值受 overflow 的影响。

**情况 A：**`overflow: visible`

子元素右侧的外边距“挂”在可滚动范围之外，不被算进 `scrollWidth`；左侧的外边距则改变了内容起点，等效地把整体宽度“多出”了 margin-left，看下面这段代码。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>scroll-demo</title>
    <link rel="stylesheet" href="./reset.css" />
    <style>
      #box {
        width: 200px;
        height: 150px;
        margin: 10px;
        background-color:burlywood;
      }
      #content {
        width: 600px;
        height: 400px;
        margin: 10px;
        background: linear-gradient(-45deg, pink, skyblue);
      }
      html {
        background-color: aliceblue;
      }
    </style>
  </head>
  <body>
    <p>scrollWidth: <span id="width">0</span>px</p>
    <p>scrollHeight: <span id="height">0</span>px</p>
    <div id="box">
      <div id="content"></div>
    </div>
    <script>
      const box = document.querySelector("#box");
      width.textContent = box.scrollWidth;
      height.textContent = box.scrollHeight;
    </script>
  </body>
</html>
```

scrollWidth = `content.(marinLeft + width)` = 610px。

**情况 B**：`overflow: hidden/auto/scroll`

此时 `#box` 建立了滚动容器。浏览器会严格按“可滚动溢出矩形”来确定滚动范围；在这种语境下，子元素的外边距盒（margin box）会参与确定滚动边界（尤其是末端方向），因此右侧的 `margin-right: 10px` 被计入。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>scroll-demo</title>
    <link rel="stylesheet" href="./reset.css" />
    <style>
      #box {
        width: 200px;
        height: 150px;
        margin: 10px;
        background-color:burlywood;
        overflow: hidden;
      }
      #content {
        width: 600px;
        height: 400px;
        margin: 10px;
        background: linear-gradient(-45deg, pink, skyblue);
      }
      html {
        background-color: aliceblue;
      }
    </style>
  </head>
  <body>
    <p>scrollWidth: <span id="width">0</span>px</p>
    <p>scrollHeight: <span id="height">0</span>px</p>
    <div id="box">
      <div id="content"></div>
    </div>
    <script>
      const box = document.querySelector("#box");
      width.textContent = box.scrollWidth;
      height.textContent = box.scrollHeight;
    </script>
  </body>
</html>
```

整体和情况 A 的代码是一样的，只是添加了 `overflow: hidden`。

scrollWidth = `content.(marginLeft + width + marginRight)` = 620px;

### scrollHeight

scrollWidth 的含义和 scrollHeight 大致相同，但是垂直方向上需要考虑外边距折叠，并且 scrollHeight 的取值同样也受 overflow 的影响。

**情况 A**：`overflow: visible`

还是以刚刚上面的代码为例：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>scroll-demo</title>
    <link rel="stylesheet" href="./reset.css" />
    <style>
      #box {
        width: 200px;
        height: 150px;
        margin: 10px;
        background-color:burlywood;
        /* overflow: hidden; */
      }
      #content {
        width: 600px;
        height: 400px;
        margin: 10px;
        background: linear-gradient(-45deg, pink, skyblue);
      }
      html {
        background-color: aliceblue;
      }
    </style>
  </head>
  <body>
    <p>scrollWidth: <span id="width">0</span>px</p>
    <p>scrollHeight: <span id="height">0</span>px</p>
    <div id="box">
      <div id="content"></div>
    </div>
    <script>
      const box = document.querySelector("#box");
      width.textContent = box.scrollWidth;
      height.textContent = box.scrollHeight;
    </script>
  </body>
</html>
```

这里 `#box` 没有创建可滚动容器，并且 margin 和 margin 之间没有阻隔。发生了垂直外边距的折叠，所以 scrollHeight 没有计算 `#content` 的 magin。

scrollHeight = content.width = 400;

**情况 B**：`overflow: hidden/auto/scroll`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>scroll-demo</title>
    <link rel="stylesheet" href="./reset.css" />
    <style>
      #box {
        width: 200px;
        height: 150px;
        margin: 10px;
        background-color:burlywood;
        overflow: hidden;
      }
      #content {
        width: 600px;
        height: 400px;
        margin: 10px;
        background: linear-gradient(-45deg, pink, skyblue);
      }
      html {
        background-color: aliceblue;
      }
    </style>
  </head>
  <body>
    <p>scrollWidth: <span id="width">0</span>px</p>
    <p>scrollHeight: <span id="height">0</span>px</p>
    <div id="box">
      <div id="content"></div>
    </div>
    <script>
      const box = document.querySelector("#box");
      width.textContent = box.scrollWidth;
      height.textContent = box.scrollHeight;
    </script>
  </body>
</html>
```

此时 `#box` 建立了滚动容器。浏览器会严格按“可滚动溢出矩形”来确定滚动范围；在这种语境下，子元素的外边距盒（margin box）会参与确定滚动边界（尤其是末端方向），因此 `margin-top: 10px & magin-bottom: 10px` 被计入。

scrollHeight = `content.(marginTop + height + marginBottom)` = 420px;

### scrollTop/Left

scrollTop 和 scrollLeft 的意义如同所示，描述的是 content 向下（右）滚动了多少。

![20250823160856](https://kat-tc.oss-cn-chengdu.aliyuncs.com/20250823160856.png)

以这个案例做演示。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>debounce</title>
    <link rel="stylesheet" href="./reset.css" />
    <style>
      #box {
        width: 200px;
        height: 150px;
        /* border: 2px solid #333; */
        overflow: auto;
      }
      #content {
        width: 600px;
        height: 400px;
        background: linear-gradient(-45deg, pink, skyblue);
      }
    </style>
  </head>

  <body>
    <div id="box">
      <div id="content"></div>
    </div>
    <p>scrollTop: <span id="top">0</span>px</p>
    <p>scrollLeft: <span id="left">0</span>px</p>
  </body>

  <script>
    const box = document.querySelector('#box');
    const topEl = document.querySelector('#top');
    const leftEl = document.querySelector('#left');

    box.addEventListener('scroll', () => {
      topEl.textContent = box.scrollTop;
      leftEl.textContent = box.scrollLeft;
    });
  </script>
</html>
```

这个案例中，box 表示的是外面包裹的这个盒子，content 是里面可以滚动的内容。scrollTop 描述 content 向下（滚动条向下移动）滚动了多少，scrollLeft 描述 content 向右滚动了多少。

## client 家族

### clientWidth/Height

`clientWidth` 和 `clientHeight` 是 DOM 元素的两个只读属性，用来返回元素的内部可见尺寸。

精确定义：返回元素的内容区宽（高）度 + 内边距（padding），不包括边框（border）、滚动条（scrollbar）、外边距（margin）。

还是以这段代码为例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>client-demo</title>
  <link rel="stylesheet" href="./reset.css">
  <style>
    #box {
      width: 200px;
      height: 150px;
      border: 2px solid #333;
      overflow: auto;
    }
    #content {
      width: 600px;
      height: 400px;
      background: linear-gradient(-45deg, pink, skyblue);
    }
  </style>
</head>
<body>
  <div id="box">
    <div id="content"></div>
  </div>
  <p>clientWidth: <span id="clientWidth">0</span>px</p>
  <p>clientHeight: <span id="clientHeight">0</span>px</p>
  <script>
    const box = document.querySelector('#box');
    clientWidth.textContent = box.clientWidth;
    clientHeight.textContent = box.clientHeight;
  </script>
</body>
</html>
```

`#box` 定义的是 200 \* 150，但是由于滚动条占了 15px，所以显示内容的区域就只剩下了 185 \* 135。

如果给 `#box` 加上 `padding: 15px`，clientWidth 和 clientHeight 会变成 215 * 165 px。

### clientTop/Left

clientTop：简单来说，就是元素上边框（border-top）的宽度；标准，表示内容区域距离可视区域（即包含 content + padding，但不包含 border、scrollbar）的顶部的偏移量。

clientLeft：同理，元素左边框（border-left）的宽度。

## offset 家族

### offsetWidth/Height

`offsetWidth` = `border-box.width`

示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>scroll-demo</title>
  <link rel="stylesheet" href="./reset.css">
  <style>
    #box {
      width: 200px;
      height: 150px;
      border: 2px solid #333;
      overflow: auto;
    }
    #content {
      width: 600px;
      height: 400px;
      background: linear-gradient(-45deg, pink, skyblue);
    }
  </style>
</head>
<body>
  <div id="box">
    <div id="content"></div>
  </div>
  <p>scrollWidth: <span id="width">0</span>px</p>
  <p>scrollHeight: <span id="height">0</span>px</p>
  <script>
    const box = document.querySelector('#box');
    width.textContent = box.offsetWidth;
    height.textContent = box.offsetHeight;
  </script>
</body>
</html>
```

这里面的 offsetWidth 和 offsetHeight 很明显就是 `#box` 本身的宽度和高度，这里描述的是 `#box` 这个容器的大小，所以加上了外边框。显示的结果就是：

```bash
offsetWidth: 204
offsetHeight: 154
```

### offsetTop/Left

描述的是元素和其最近的祖先定位元素（`relative | absolute | fixed`）之间的距离关系。

定量（以父子元素为例）：子元素 margin-top + 父元素 padding-top = offsetTop

```html
<div
  id="parent"
  style="
    position: relative;
    border: 1px solid red;
    padding: 20px;
    background-color: oldlace;
  "
>
  <div
    id="child"
    style="
      margin-top: 30px;
      border: 1px solid red;
      padding: 15px;
      height: 50px;
      background-color: lightblue;
    "
  ></div>
  <div
    id="child2"
    style="
      margin-top: 30px;
      border: 1px solid red;
      padding: 15px;
      height: 50px;
      background-color: lightblue;
    "
  ></div>
</div>
<script>
  console.log(child.offsetTop); // 50
  console.log(child2.offsetTop); // 162
</script>
```

以上面这个例子为例：

`child.offsetTop` = `parent.padding-top` + `child.margin-top` = 20 + 30 = 50

`child2.offsetTop` = `parent.padding-top` + `child1.totalHeight` + `child2.margin-top` = 20 + 112 + 30 = 162
