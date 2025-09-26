# 自定义属性

## 简介

* 写法：`--变量名: 值;`
* 定义在选择器里，就相当于给这个选择器作用域内的元素 **挂了一个变量**。
* 使用时通过 `var(--变量名)` 取值。

举个最简单的例子：

```css
:root {
  --main-color: #409eff;
  --padding-base: 12px;
}

.button {
  color: var(--main-color);
  padding: var(--padding-base);
}
```

这里 `--main-color` 和 `--padding-base` 就是自定义属性，浏览器渲染时会替换成具体的值。

## 作用域与继承

* 自定义属性也遵循 **CSS 层叠与继承规则**。
* 如果在 `:root` 定义，相当于全局变量；
* 如果在某个元素上定义，则只在该元素及子元素中生效。

```css
:root {
  --text-color: black;
}

.dark {
  --text-color: white; /* 只在 .dark 元素及子树里覆盖 */
}

p {
  color: var(--text-color);
}
```

在 `.dark` 容器里的 `<p>` 会变白色，其它地方还是黑色。

## var

* 用来读取自定义属性的值。
* 语法：`var(--name, fallback)`

  * `--name`：变量名。
  * `fallback`（可选）：当变量未定义时使用的默认值。

例子：

```css
.box {
  color: var(--title-color, red); /* 如果没定义 --title-color，就用 red */
}
```
