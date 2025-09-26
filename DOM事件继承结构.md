# DOM 事件继承结构

```text
EventTarget（不是事件类，是能触发事件的对象，比如元素、window、document）
    ↑
Event        // 所有事件的基类
    ↑
UIEvent      // 用户界面事件（键盘、鼠标、窗口大小变化等）
    ↑
MouseEvent   // 鼠标事件（点击、移动、滚轮等）
        ↑
PointerEvent // 更通用的指针事件（鼠标、触摸笔等，PointerEvent 是 MouseEvent 的扩展）
```
