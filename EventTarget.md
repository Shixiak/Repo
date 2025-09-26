# EventTarget

`EventTarget` 是 Web API 事件模型中的最基础接口，几乎所有能够接收事件的对象都继承自它。

## 提供的方法

`EventTarget` 定义了事件处理的核心方法：

1. `addEventListener(type, listener, options)`
   * 注册一个事件监听器。
   * 参数：
     * `type`：事件类型（如 `"click"`、`"keydown"`）。
     * `listener`：回调函数。
     * `options`：布尔值或对象，指定 `capture`、`once`、`passive` 等配置。
2. `removeEventListener(type, listener, options)`
   * 移除事件监听器，参数必须与 `addEventListener` 注册时保持一致。
