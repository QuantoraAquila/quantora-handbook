# DOM 节点类型与常用属性方法汇总

## 🧱 一、DOM 节点类型总览（继承关系）

```text
Node（所有 DOM 节点的基类）
├── Document （整个页面的根）
│   └── HTMLDocument
│       └── document
├── Element（HTML 元素）
│   └── HTMLElement
│       ├── HTMLHtmlElement（<html>）
│       ├── HTMLBodyElement（<body>）
│       ├── HTMLDivElement（<div>）
│       ├── HTMLParagraphElement（<p>）
│       └── ...
├── Text（文本节点）
├── Comment（注释节点）
└── DocumentFragment（虚拟节点）
```

---

## 📌 二、各类节点及其常用属性方法

### 📄 1. Node（所有 DOM 节点的祖先类）

适用于所有节点：Document、Element、Text、Comment 等

| 属性/方法                             | 描述                                |
| --------------------------------- | --------------------------------- |
| `nodeType`                        | 节点类型编号（1 元素，3 文本，8 注释，9 Document） |
| `nodeName`                        | 节点名称（如 `DIV`, `#text`）            |
| `parentNode`                      | 父节点                               |
| `childNodes`                      | 所有子节点（包括文本节点）                     |
| `firstChild` / `lastChild`        | 第一个/最后一个子节点                       |
| `nextSibling` / `previousSibling` | 相邻兄弟节点                            |
| `cloneNode(deep)`                 | 克隆节点                              |
| `remove()`                        | 移除当前节点                            |

---

### 📄 2. Document（网页文档对象）

文档对象 `document` 是操作网页的入口。

| 方法                         | 描述               |
| -------------------------- | ---------------- |
| `getElementById(id)`       | 根据 ID 查找         |
| `getElementsByClassName()` | 根据 class 查找      |
| `getElementsByTagName()`   | 根据标签名查找          |
| `querySelector(css)`       | CSS 选择器查找第一个匹配元素 |
| `querySelectorAll(css)`    | 查找所有匹配元素         |
| `createElement(tag)`       | 创建元素节点           |
| `createTextNode(text)`     | 创建文本节点           |

---

### 📄 3. Element（标签元素）

如 `<div>`, `<p>`, `<button>`，等 HTML 标签节点。

#### 属性：

| 属性            | 描述                 |
| ------------- | ------------------ |
| `innerHTML`   | 元素内部的 HTML 内容（可读写） |
| `outerHTML`   | 包含当前元素本身的 HTML 字符串 |
| `textContent` | 文本内容（包括隐藏文本）       |
| `innerText`   | 可见的文本内容            |
| `id`          | 元素的 ID             |
| `className`   | 类名                 |
| `tagName`     | 标签名（如 DIV, P）      |
| `attributes`  | 所有属性集合             |
| `dataset`     | 所有 `data-*` 自定义数据  |

#### 方法：

| 方法                          | 描述          |
| --------------------------- | ----------- |
| `setAttribute(name, value)` | 设置属性        |
| `getAttribute(name)`        | 获取属性        |
| `removeAttribute(name)`     | 移除属性        |
| `appendChild(node)`         | 添加子节点       |
| `removeChild(node)`         | 删除子节点       |
| `replaceChild(new, old)`    | 替换子节点       |
| `insertBefore(new, ref)`    | 在某个节点前插入新节点 |

---

### 📄 4. HTMLElement（具体 HTML 元素）

HTML 标签元素如 `<input>`, `<select>`, `<form>` 等提供交互属性：

| 标签         | 常用属性                                   |
| ---------- | -------------------------------------- |
| `<input>`  | `value`, `type`, `checked`, `disabled` |
| `<select>` | `value`, `selectedIndex`, `options`    |
| `<form>`   | `submit()`, `reset()`                  |

---

### 🧾 5. Text（文本节点）

```html
<p>Hello</p>
```

Hello 是一个 `Text` 节点。

| 属性 / 方法       | 描述              |
| ------------- | --------------- |
| `textContent` | 文本内容            |
| `nodeValue`   | 等同于 textContent |
| `parentNode`  | 父节点（一般是元素）      |

---

### 💬 6. Comment（注释节点）

```html
<!-- 这是注释 -->
```

| 属性            | 描述     |
| ------------- | ------ |
| `nodeValue`   | 注释文本内容 |
| `textContent` | 同上     |
| `parentNode`  | 父节点    |

---

### 🧩 7. DocumentFragment（文档片段）

用于批量操作 DOM，不会引起重排，提高性能。

```js
const frag = document.createDocumentFragment()
frag.appendChild(document.createElement("li"))
ul.appendChild(frag)
```

| 优点 | 描述              |
| -- | --------------- |
| 轻量 | 不是真实 DOM，不会重新渲染 |
| 效率 | 可用于构建多个节点后一次性插入 |

---

## 🧠 总结图示（简化）

```text
Node
├── Document
│   └── HTMLDocument（document）
├── Element
│   └── HTMLElement（div, p, body 等）
├── Text
├── Comment
└── DocumentFragment
```

> 所有 DOM 节点都继承自 Node，每类节点根据职责，拥有专属属性与方法。
