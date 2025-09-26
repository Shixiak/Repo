# DOM 接口

## 概要

```text
EventTarget   // 能接收事件和监听器的基类
   ↓
Node          // DOM 树中所有节点的基类
   ↓
Element       // 所有元素节点的基类
   ↓
HTMLElement   // 所有 HTML 元素节点的基类
       ↓
具体 HTML 元素类（HTMLDivElement, HTMLParagraphElement, HTMLButtonElement, …）
```

## Element

浏览器中所有 HTML 元素（`<div>`、`<span>`、`<p>` 等）和 SVG 元素（`<svg>` 等）的共同基类。

在这个类型声明里：

```ts
let headers: DOMWrapper<Element>[], contents: DOMWrapper<Element>[];
```

它不是 Vue 特有的类型，而是 **TypeScript 内置在 `lib.dom.d.ts` 里的 DOM 类型**。

* `DOMWrapper<Element>`：Vue Test Utils 提供的一个 **封装器类**，用来包裹原生 DOM 元素。
* `<Element>`：这个封装器里包裹的 **实际 DOM 元素的类型**。

  * 具体类型可以是 `HTMLDivElement`、`HTMLSpanElement`、`HTMLButtonElement` 等。
  * 如果不指定更具体的类型，就用通用的 `Element` 作为泛型参数。
