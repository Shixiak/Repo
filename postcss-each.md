# postcss-each

## 配置

在 `postcss.config.js` 补充以下内容。

```js
import each from 'postcss-each';

export default {
    plugins: [
        each()
    ]
}
```

## 用例

```scss
@each $color in (red, green, blue) {
  .bg-$(color) {
    background-color: $(color);
  }
}
```

变量名必须以 $ 开头，比如这里的 `$val`。
