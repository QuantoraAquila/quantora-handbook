# JavaScript 执行上下文与程序执行机制详解

本文件系统性地介绍 JavaScript 程序在运行前后的执行机制，包括执行上下文、调用栈、嵌套函数调用、变量提升等概念，并通过具体示例与形象类比加以说明。

---

## 📌 一、什么是执行上下文（Execution Context）？

JavaScript 在执行任何代码之前，都会创建一个“执行上下文”对象，它包含当前代码执行所需的所有信息：

- **变量环境（Variable Environment）**
- **词法环境（Lexical Environment）**
- **this 绑定（this Binding）**

### 执行上下文的类型：

| 类型                 | 说明                             | 创建时机                |
|----------------------|----------------------------------|-------------------------|
| 全局执行上下文       | 包含全局代码和全局对象           | 程序启动时只创建一次    |
| 函数执行上下文       | 函数执行时创建                   | 每次函数调用都会创建    |
| eval 执行上下文      | 执行 eval 代码时创建（不推荐）   | 调用 `eval()` 时        |

---

## 🔁 二、程序是如何“执行”的？

JavaScript 程序的执行是**基于函数调用展开的**，每次函数被调用，都会：

- 创建一个新的函数执行上下文
- 压入“执行上下文栈”（Call Stack）顶部
- 执行代码，执行完后弹出

这种执行机制在结构上**类似递归展开**，即使代码没有显式递归，调用链依然形成一个栈式结构。

---

## 🧠 三、执行上下文栈（Call Stack）

JavaScript 引擎通过一个**执行上下文栈（EC Stack）**来管理函数调用过程。

### 示例：

```js
function f1() {
  f2();
}

function f2() {
  f3();
}

function f3() {
  console.log("Done");
f1();
```
调用过程中的执行上下文栈变化：
```
1. f1 调用时：
   [f1]
   [Global]

2. f2 调用时：
   [f2]
   [f1]
   [Global]

3. f3 调用时：
   [f3]
   [f2]
   [f1]
   [Global]

4. 函数逐层返回，栈逐层弹出
```
## 🧪 四、全局执行上下文不是反复创建！
1. 全局执行上下文（Global Execution Context）只创建一次，在程序最开始运行时建立。

2. 函数每次调用才会新建“函数执行上下文”。

否则，每次函数调用都“重置全局变量”，程序将无法维护状态。

## 📦 五、形象比喻：JS 程序就像“开房栈”
- 全局代码：开了第一间房（全局上下文）

- 每调用一个函数：再开一间房（函数上下文）

- 每个房间有自己的储物柜（变量表）

- 使用变量时，从当前房间查不到，就往上一层查（作用域链）

## 🔍 六、示例代码 + 上下文分析
```
var a = 1;

function outer() {
  var b = 2;

  function inner() {
    var c = 3;
    console.log(a + b + c);
  }

  inner();
}

outer();
```
创建阶段分析：
全局执行上下文（Global Execution Context）：
``` 
{
  VariableEnvironment: { a: undefined },
  LexicalEnvironment: { outer: Function },
  thisBinding: window
}
```
outer() 执行上下文（Function Execution Context）：
```
{
  VariableEnvironment: { b: undefined },
  LexicalEnvironment: { inner: Function },
  Outer: Global
}
```
inner() 执行上下文：
```
{
  VariableEnvironment: { c: undefined },
  LexicalEnvironment: {},
  Outer: outer()
}
```
作用域链查找：
- 找 c → 当前上下文

- 找 b → outer 的上下文

- 找 a → 全局上下文

输出结果：
```
1 + 2 + 3 = 6
```
## 🧱 七、构成执行上下文的结构
每个执行上下文都包含以下内容：

- 变量环境（VE）：包含 var 声明和函数声明

- 词法环境（LE）：包含 let、const、函数参数

- 作用域链（Scope Chain）：由外部引用形成

- this 绑定

## 🚧 八、变量提升与暂时性死区
- var 和 function 会被“提升”，但 var 初始化为 undefined

- let 和 const 不会被提升，存在“暂时性死区（TDZ）”

``` 
console.log(a); // undefined
var a = 1;

console.log(b); // ReferenceError
let b = 2;
```

# JavaScript 核心概念梳理：全局对象、执行上下文对象、作用域、作用域链

---

## 1. 全局对象（Global Object）

- **定义**：在 JavaScript 运行环境中，最顶层的对象。
- **作用**：
  - 浏览器环境中，全局对象是 `window`。
  - Node.js 环境中，全局对象是 `global`。
  - 全局变量和全局函数实际上是全局对象的属性和方法。
- **示例**：

```js
var a = 10;
console.log(window.a);  // 浏览器环境中输出 10
```
## 2. 执行上下文对象（Execution Context）
- **定义**：代码执行时的环境抽象，包含变量对象、作用域链、this绑定等信息。

- **分类**：

  - 全局执行上下文：代码首次执行时创建，且只有一个。

  - 函数执行上下文：函数调用时创建，每次调用都会创建新的。

  - Eval执行上下文：执行 eval 代码时创建。

- **执行上下文包含内容**:

  - 变量对象（Variable Object）

  - 作用域链（Scope Chain）

  - this绑定

## 3. 作用域（Scope）
- **定义**：变量和函数的可访问范围。

- **类型**：

  - 全局作用域：程序的最外层，所有全局变量和函数所在的位置。

  - 函数作用域：函数内部形成的作用域。

  - 块级作用域（ES6 引入）：由 {} 包裹的代码块形成，适用于 let 和 const。

- **特点**：

  - 作用域在代码编写时确定（词法作用域）。

  - 决定了变量的生命周期和可访问性。

## 4. 作用域链（Scope Chain）
- **定义**：当前执行上下文中所有作用域对象的链式结构。

- **作用**：

  - 当访问一个变量时，会从当前作用域开始查找，依次向上查找父级作用域，直到全局作用域。

  - 如果所有作用域中都找不到，则抛出 ReferenceError。

- **形成原因**：

  - 作用域是静态的，但执行环境是动态的，作用域链保证了变量的正确查找。

## 5. 概念之间的关系总结
| 概念      | 含义               | 关系与区别              |
|---------|------------------|--------------------|
| 全局对象    | 运行环境中最顶层的对象      | 全局变量是全局对象的属性       |
| 执行上下文对象 | 代码执行时的环境封装       | 包含变量对象、作用域链、this绑定 |
| 作用域     | 变量和函数的可访问范围      | 决定变量在哪些代码块里可访问     |
| 作用域链    | 当前执行上下文可访问的作用域链条 | 变量查找时依赖作用域链层层查找变量  |

## 6. 示例说明
```
var globalVar = '全局变量';

function outer() {
  var outerVar = '外层变量';

  function inner() {
    var innerVar = '内层变量';
    console.log(globalVar); // 查找全局作用域，输出 '全局变量'
    console.log(outerVar);  // 查找外层函数作用域，输出 '外层变量'
    console.log(innerVar);  // 当前函数作用域，输出 '内层变量'
  }

  inner();
}

outer();
```
- 变量查找过程：innerVar → outerVar → globalVar

- 作用域链就是 inner → outer → 全局

# JS 和 Python 的执行模型对比

---

## 🧠 总体结论：类似的概念，不同的实现
| 机制/术语           | JavaScript                | Python                  | 相似性评估    |
| --------------- | ------------------------- | ----------------------- | -------- |
| **执行上下文**       | 每次执行（全局 / 函数）都会创建         | 每个函数调用时创建调用帧（frame）     | ✅ 类似     |
| **调用栈**         | Call Stack                | 调用帧栈（Call Stack）        | ✅ 类似     |
| **作用域链**        | 词法作用域 + 作用域链向上查找变量        | 词法作用域 + 闭包自由变量查找（LEGB）  | ✅ 类似     |
| **全局对象**        | 浏览器：window，JS 运行环境相关      | Python：`__main__` 模块作用域 | ⚠️ 不完全相同 |
| **this 绑定机制**   | 动态绑定，依赖调用方式               | `self` 明确传入，不依赖调用者      | ❌ 完全不同   |
| **变量声明方式**      | `var`/`let`/`const`，有提升机制 | 所有变量都有作用域限制，无提升         | ❌ 不同     |
| **闭包（closure）** | 支持闭包                      | 支持闭包，内部自由变量通过 cell 存储   | ✅ 相似     |

## 🔁 一、函数执行与上下文机制
### JavaScript：
- 每次函数调用 → 创建一个 Execution Context

- 压入 Call Stack

- 包含：变量环境、词法环境、this、作用域链

### Python：
- 每次函数调用 → 创建一个 Frame（帧）对象

- 压入 调用栈（Call Stack）

- 包含：局部变量字典、本地命名空间、自由变量、闭包

✅ 所以这部分**是类似的机制，只是结构不完全一样**。

## 📦 二、作用域链机制对比
### JavaScript：使用“词法作用域链”
```
js

function outer() {
  let a = 10;
  function inner() {
    console.log(a); // 找不到 a 就往上找
  }
  inner();
}
```
### Python：使用“LEGB 查找顺序”
```
python

def outer():
    a = 10
    def inner():
        print(a)  # 找不到 a，就 LEGB 查找
    inner()

# L -> Local
# E -> Enclosing
# G -> Global
# B -> Built-in
```
✅ 非常相似！都采用向外查找变量的模型。

## ❗ 三、this vs self —— 最大差异
| 语言         | 特性                            |
| ---------- | ----------------------------- |
| JavaScript | `this` 是自动注入的变量，调用时动态绑定       |
| Python     | `self` 是函数第一个参数，**明确传入的对象引用** |


🔍 示例：
```
js

const obj = {
  name: "JS",
  say() {
    console.log(this.name);
  }
};
obj.say(); // "JS"
```
```
python

class MyObj:
    def __init__(self, name):
        self.name = name

    def say(self):
        print(self.name)

obj = MyObj("Python")
obj.say()  # "Python"
```
📌 结论：

- Python 的 self 不搞“魔法”，直接传；

- JS 的 this 是黑魔法，可能变、容易错。

## 🔐 四、闭包支持对比
都支持闭包，但细节差异：
### JavaScript：
```
js

function outer() {
  let a = 10;
  return function() {
    console.log(a);
  };
}
const fn = outer();
fn(); // 10
```
### Python：
```
python

def outer():
    a = 10
    def inner():
        print(a)
    return inner

fn = outer()
fn()  # 10
```
✅ 闭包结构相似，但 Python 内部会用 cell 对象（闭包对象） 来保存外部变量。

## ✅ 总结图示对比（结构上）
```
scss

    JS 调用栈：                         Python 调用栈：
 ┌─────────────────┐               ┌─────────────────┐
 │  inner() Context │               │  inner() Frame   │
 ├─────────────────┤               ├─────────────────┤
 │  outer() Context │               │  outer() Frame   │
 ├─────────────────┤               ├─────────────────┤
 │  Global Context  │               │  __main__ Frame  │
 └─────────────────┘               └─────────────────┘
```
# 📘 总结：一句话对比结论
| 特性           | JavaScript                   | Python                  |
| ------------ | ---------------------------- | ----------------------- |
| 全局对象         | `window`（浏览器），`global`（Node） | 没有全局对象，`__main__` 模块作用域 |
| 作用域模型        | 词法作用域 + 作用域链                 | LEGB 模型                 |
| this vs self | `this` 是上下文绑定的               | `self` 是显式传入            |
| 闭包           | 支持，内部词法作用域链维护                | 支持，用 cell 对象记录外部变量      |
| 调用栈结构        | 执行上下文栈                       | 调用帧栈（frame stack）       |
