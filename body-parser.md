# body-parser

## 作用

这个中间件的作用是用来解析 post 请求表单中的数据. 并把解析后的数据放到 req.body 中.

## 配置

body-parser 配置.

```bash
ubuntu@VM-24-17-ubuntu:/var/www/badminton-reservation/deploy-backend$ node ./dist/app.js 
body-parser deprecated undefined extended: provide extended option dist/app.js:10:27
Server running at http://localhost:5001
```

意思就是这种写法已经弃用了.

```js
app.use(express.urlencoded())
```

应该传入非空的配置:

```js
app.use(express.urlencoded({ extended: true }))
```

这个配置的作用是让 urlencoded 支持解析更复杂的嵌套结构.
