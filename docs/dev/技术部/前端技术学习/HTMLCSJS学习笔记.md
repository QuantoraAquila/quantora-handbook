# HTML & CSS & JS 学习指南

本指南通过案例的方式总结 HTML、CSS 和 JavaScript 的重点内容与常用方法，适合作为初学者学习和查阅使用。

---

## 📄 HTML 部分

### 1. HTML 基础结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>示例页面</title>
</head>
<body>
  <h1>欢迎来到我的网站</h1>
  <p>这是一个段落。</p>
</body>
</html>
```

**重点说明：**

* `<!DOCTYPE html>` 声明文档类型
* `<html>` 是根标签，包含整个网页内容
* `<head>` 部分用于定义页面信息，如标题、编码等
* `<body>` 为页面主要内容部分

---

### 2. 常用标签汇总

| 标签                     | 功能描述    | 示例                                     |
| ---------------------- | ------- | -------------------------------------- |
| `<h1>` \~ `<h6>`       | 标题标签    | `<h1>主标题</h1>`                         |
| `<p>`                  | 段落      | `<p>这是段落</p>`                          |
| `<a>`                  | 超链接     | `<a href="https://example.com">链接</a>` |
| `<img>`                | 图片      | `<img src="image.jpg" alt="描述">`       |
| `<ul>`, `<ol>`, `<li>` | 无序/有序列表 | `<ul><li>项</li></ul>`                  |
| `<div>`                | 区块容器    | `<div>内容</div>`                        |
| `<span>`               | 行内容器    | `<span>行内</span>`                      |
| `<input>`              | 输入框（表单） | `<input type="text">`                  |
| `<button>`             | 按钮      | `<button>提交</button>`                  |

---

### 3. 表单示例

```html
<form action="/submit" method="post">
  <label for="name">姓名：</label>
  <input type="text" id="name" name="name">
  <button type="submit">提交</button>
</form>
```

---

## 🎨 CSS 部分

### 1. CSS 使用方式

```html
<!-- 内联样式 -->
<p style="color: red;">红色文本</p>

<!-- 内部样式 -->
<style>
  p {
    color: blue;
  }
</style>

<!-- 外部样式 -->
<link rel="stylesheet" href="style.css">
```

---

### 2. 常用选择器

| 选择器      | 说明      | 示例                 |
| -------- | ------- | ------------------ |
| `*`      | 全部元素    | `* { margin: 0; }` |
| `tag`    | 标签选择器   | `p {}`             |
| `.class` | 类选择器    | `.box {}`          |
| `#id`    | ID选择器   | `#main {}`         |
| `A B`    | 后代选择器   | `div p {}`         |
| `A > B`  | 子元素选择器  | `ul > li {}`       |
| `A + B`  | 相邻兄弟选择器 | `h1 + p {}`        |
| `[attr]` | 属性选择器   | `[type="text"] {}` |

---

### 3. 盒模型 Box Model

```css
.box {
  width: 200px;
  height: 100px;
  padding: 10px;
  border: 1px solid black;
  margin: 20px;
}
```

**说明：** 内容 (content) + 内边距 (padding) + 边框 (border) + 外边距 (margin)

---

### 4. 布局方式

#### Flex 布局

```css
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

#### Grid 布局

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}
```

---

### 5. 响应式设计

```css
@media (max-width: 600px) {
  body {
    background-color: lightgray;
  }
}
```

---

## ✨ JavaScript 部分

### 1. 基础用法

```html
<script>
  console.log('Hello World');
</script>
```

也可以通过外部文件引入：

```html
<script src="script.js"></script>
```

---

### 2. 常见操作：获取元素、事件绑定、DOM 操作

```html
<button id="btn">点击我</button>
<p id="output"></p>

<script>
  const btn = document.getElementById('btn');
  const output = document.getElementById('output');

  btn.addEventListener('click', () => {
    output.textContent = '你点击了按钮！';
  });
</script>
```

---

### 3. 修改样式

```html
<div id="box" style="width:100px; height:100px; background:red;"></div>
<button onclick="changeColor()">换颜色</button>

<script>
  function changeColor() {
    document.getElementById('box').style.background = 'blue';
  }
</script>
```

---

### 4. 表单验证示例

```html
<form onsubmit="return validateForm()">
  <input type="text" id="name">
  <button type="submit">提交</button>
</form>
<script>
  function validateForm() {
    const name = document.getElementById('name').value;
    if (!name) {
      alert('请输入姓名');
      return false;
    }
    return true;
  }
</script>
```

---

### 5. JavaScript 关键特性

#### 变量与作用域

```js
let a = 1;       // 块作用域
const b = 2;     // 常量
var c = 3;       // 函数作用域（不推荐）
```

#### 函数

```js
function add(x, y) {
  return x + y;
}

const multiply = (x, y) => x * y;
```

#### 条件与循环

```js
if (x > 0) {
  ...
} else {
  ...
}

for (let i = 0; i < 10; i++) {
  ...
}

while (condition) {
  ...
}
```

#### 数组与对象

```js
const arr = [1, 2, 3];
arr.forEach(item => console.log(item));

const obj = { name: 'Tom', age: 20 };
console.log(obj.name);
```

#### 模板字符串

```js
const name = 'Alice';
console.log(`Hello, ${name}`);
```

#### 解构与展开

```js
const [a, b] = [1, 2];
const { name, age } = obj;
const newArr = [...arr, 4];
```

#### 异步与 Promise

```js
function getData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve('done'), 1000);
  });
}

async function run() {
  const result = await getData();
  console.log(result);
}
run();
```

---

## 🔁 JS vs Python 对比速查表

| 概念      | JavaScript 示例                          | Python 示例                     | 说明                     |
| ------- | -------------------------------------- | ----------------------------- | ---------------------- |
| 变量声明    | `let x = 1;` / `const y = 2;`          | `x = 1`                       | JS 推荐用 `let` 或 `const` |
| 函数定义    | `function add(a, b) { return a + b; }` | `def add(a, b): return a + b` | JS 用花括号 `{}`           |
| 箭头函数    | `const add = (a, b) => a + b;`         | `add = lambda a, b: a + b`    | JS 更简洁但无自己的 `this`     |
| 条件语句    | `if (x > 1) { ... }`                   | `if x > 1:`                   | JS 用括号和花括号             |
| 循环      | `for (let i = 0; i < 5; i++) {}`       | `for i in range(5):`          | JS 类似 C 风格             |
| 对象 / 字典 | `{name: "Tom"}`                        | `{"name": "Tom"}`             | 结构类似                   |
| 模板字符串   | `` `Hi, ${name}` ``                    | `f"Hi, {name}"`               | JS 用反引号 `` ` ``        |

---

## ✅ 综合案例：交互式个人介绍页面

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>我的介绍</title>
  <style>
    body { font-family: sans-serif; text-align: center; }
    #intro { display: none; margin-top: 20px; }
  </style>
</head>
<body>
  <h1>你好，我是李四</h1>
  <button onclick="showIntro()">显示介绍</button>
  <p id="intro">我喜欢编程和设计网页。</p>

  <script>
    function showIntro() {
      document.getElementById('intro').style.display = 'block';
    }
  </script>
</body>
</html>
```

---

## 📚 建议学习路线

1. 掌握 HTML 基本结构与常用标签
2. 理解 CSS 的引入方式和选择器
3. 掌握盒模型和常见布局（Flex/Grid）
4. 学习 JavaScript 基础语法与事件绑定
5. 理解 JavaScript 的关键特性（函数、对象、数组、异步等）
6. 制作动态交互网页（如按钮点击、输入校验）
7. 综合实践：制作 Todo 应用、登录页、个人简历页

> 制作：ChatGPT x Quantora
