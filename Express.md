# Express

## 中间件

### express.json()

我们的测试代码是这样的：

```ts
app.use(express.json());  // 使用中间件

app.post('/test', (req: Request, res: Response) => {
    console.log('收到的请求体：', req.body);
    res.json({ message: "收到信息", data: req.body });
});
```

这个路由能够正确解析这种请求：

```http
POST /test
Content-Type: application/json

{
	"name": "test",
	"msg": "hello"
}
```

中间件的 json 格式 对应的是请求体的 json 格式。Content-Type 请求头字段的值为 `application/json`。

### express.urlencoded()

测试代码：

```ts
app.use(express.urlencoded());

app.post('/test-urlencoded', (req:Request, res: Response) => {
    console.log('收到消息：', req.body);
    res.json({ message: '收到信息', data: req.body });
});
```

测试请求：

```http
POST /test-urlencoded
Content-Type: application/x-www-urlencoded

name=test&message=hello
```

这种格式的请求如果中间有空格要替换成 `%20`，即 url 编码。