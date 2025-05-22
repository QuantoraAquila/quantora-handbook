# JS 与 Python 对比学习笔记

本笔记以 Python 为基础语言，对比学习 JavaScript 的语法特性与核心概念，帮助初学者更高效地掌握 JS。

---

## 📌 基础语法对比

| 功能   | Python   | JavaScript                    | 对比说明                                |
| ---- | -------- | ----------------------------- | ----------------------------------- |
| 变量声明 | `x = 1`  | `let x = 1;` / `const x = 1;` | JS 使用 `let`/`const` 声明，强烈推荐避免 `var` |
| 代码块  | `:` + 缩进 | `{}` 花括号                      | JS 使用大括号，Python 用缩进                 |
| 注释   | `# 注释`   | `// 单行注释` 或 `/* 多行 */`        | 语法差异明显                              |

## 🧠 函数定义与使用

### Python：

```python
def add(a, b):
    return a + b
```

### JavaScript：

```js
function add(a, b) {
  return a + b;
}

// 箭头函数
const add = (a, b) => a + b;
```

* JS 支持函数声明和函数表达式
* 箭头函数没有自己的 `this`

## 🔁 条件与循环语句

| 功能       | Python               | JavaScript                       |
| -------- | -------------------- | -------------------------------- |
| 条件语句     | `if x > 1:`          | `if (x > 1) {}`                  |
| for 循环   | `for i in range(5):` | `for (let i = 0; i < 5; i++) {}` |
| while 循环 | `while x < 10:`      | `while (x < 10) {}`              |

* JS 的 for 循环是类 C 语言风格
* JS 的逻辑运算符与 Python 相同（`&&`, `||`, `!`）

## 📦 数据类型与结构

| 类型      | Python            | JavaScript                           | 说明             |
| ------- | ----------------- | ------------------------------------ | -------------- |
| 数字      | `int`, `float`    | `Number`                             | JS 只有一个数字类型    |
| 字符串     | `'abc'`, `"abc"`  | `'abc'`, `"abc"`, <code>`abc`</code> | JS 支持模板字符串     |
| 列表 / 数组 | `[1, 2, 3]`       | `[1, 2, 3]`                          | 类似             |
| 字典 / 对象 | `{"name": "Tom"}` | `{name: "Tom"}`                      | JS 对象键名不加引号也可以 |
| 布尔      | `True`, `False`   | `true`, `false`                      | 注意大小写          |

## 🔧 解构与展开

### Python：

```python
a, b = [1, 2]
```

### JavaScript：

```js
const [a, b] = [1, 2];
const {name, age} = {name: 'Tom', age: 20};
const arr2 = [...arr, 4];
```

* Python 的解包在 JS 中称为“解构赋值”
* 展开操作符 JS 使用 `...`

## 📜 字符串与模板语法

| 功能 | Python            | JavaScript            |
| -- | ----------------- | --------------------- |
| 拼接 | `'Hello ' + name` | `'Hello ' + name`     |
| 模板 | `f"Hello {name}"` | `` `Hello ${name}` `` |

JS 的模板字符串使用反引号 `` ` `` 包裹，支持嵌入变量 `${}`

## 📚 异步编程对比

### Python：

```python
import asyncio

async def fetch():
    await asyncio.sleep(1)
    return "done"
```

### JavaScript：

```js
function fetch() {
  return new Promise(resolve => {
    setTimeout(() => resolve("done"), 1000);
  });
}

async function run() {
  const result = await fetch();
  console.log(result);
}
```

* 两者都支持 `async/await`
* JS 使用 Promise 作为基础异步构建块

## 🔍 类型检查与转换

| 功能    | Python       | JavaScript                   |
| ----- | ------------ | ---------------------------- |
| 获取类型  | `type(x)`    | `typeof x`                   |
| 转换字符串 | `str(x)`     | `String(x)` 或 `x.toString()` |
| 转换整数  | `int("123")` | `parseInt("123")`            |

---

## ✅ 小结

* JS 在语法上更接近 C 系语言（如 Java、C++），使用大括号、分号等
* Python 更注重可读性，强调缩进
* JS 特别适合浏览器交互式网页开发
* 通过对比学习能更好地掌握语言间的差异与共同点

> 编写：ChatGPT x Quantora
