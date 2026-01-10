# JS


## 一、JavaScript 简介

JavaScript 是一种运行在浏览器中的脚本语言，也可在服务器端（如 Node.js）运行。它的主要作用是让网页“动起来”。

✅ JS 的三大组成部分：

| 组成                         | 说明         | 示例            |
| -------------------------- | ---------- | ------------- |
| ECMAScript                 | JS 的核心语法标准 | 变量、函数、流程控制    |
| DOM（Document Object Model） | 操作网页结构     | 改变 HTML 内容、样式 |
| BOM（Browser Object Model）  | 操作浏览器对象    | 弹窗、定时器、历史记录   |
---
## 二、JS基础
JavaScript 的基础也可以类似 Python 那样分成 语言核心 和 标准库 两部分。

### 2.1 语言核心（Language Core）

这是 JavaScript 本身的语法规则和基本行为，是写 JS 代码必须掌握的内容：

- 语法与关键字：if/else、for/while、function、class、async/await、switch 等

- 数据类型：Number、String、Boolean、BigInt、Symbol、null、undefined、对象类型

- 运算符和表达式：算术、逻辑、位运算、空值合并 ??、可选链 ?. 等

- 控制流程与错误处理：try/catch/finally、throw、return

- 函数、闭包、作用域和对象模型：作用域链、原型链、this 指向、箭头函数、生成器函数

**特点**：语言核心决定 JS 代码的基本行为，独立于浏览器或 Node 环境，所有 JS 引擎都必须实现。
#### 2.1.1 语法与关键字

#### 2.1.2 数据类型
##### 1、变量定义
声明变量的三种方式

    var a = 10;   // 旧语法，可重复声明，作用域不安全（应避免）
    let b = 20;   // 推荐：块级作用域，不可重复声明
    const c = 30; // 常量，不可修改


##### 2、基本类型（7 种）：

| 类型        | 示例                            | 说明           |
| --------- | ----------------------------- | ------------ |
| Number    | 42, 3.14                      | 数字（整数/浮点数）   |
| String    | `'hello'`, `"world"`, `模板字符串` | 字符串          |
| Boolean   | true, false                   | 布尔值          |
| Null      | null                          | 空值（对象为空）     |
| Undefined | undefined                     | 未定义          |
| Symbol    | Symbol('id')                  | 唯一标识符（ES6）   |
| BigInt    | 123n                          | 超大整数（ES2020） |

##### 3、引用类型
- Object（对象）
- Array（数组）
- Function（函数）
- Date、Map、Set 等等

#### 2.1.3 运算符和表达式
##### 1、运算符

| 类型    | 示例                                 |
| ----- |------------------------------------|
| 算术运算符 | `+ - * / % **`                     |
| 比较运算符 | `== === != !== > < >= <=`          |
| 逻辑运算符 | `&&                       \| \| !` |
| 赋值运算符 | `= += -= *= /=`                    |
| 三元运算符 | `条件 ? 值1 : 值2`                     |

**注意：**
    
    #js
    '5' == 5   // true（类型转换后相等）
    '5' === 5  // false（类型不同）
👉 实际开发中推荐使用 ===。

#### 2.1.4 控制流程与错误处理

#### 2.1.5 函数、作用域、闭包、对象模型、this 指向、箭头函数、生成器函数。
##### 1、函数
###### 函数的定义方式

    // 函数声明
    function foo() {}
    
    // 函数表达式
    const bar = function () {}
    
    // 箭头函数
    const baz = () => {}
    
    // 构造器创建函数（不推荐）
    const fn = new Function('a', 'b', 'return a + b')

######  函数是“一等公民”

JS 中函数可以：
- ✔ 赋值给变量
- ✔ 作为参数
- ✔ 作为返回值
- ✔ 存储在对象中

这也是闭包和高阶函数得以实现的基础。

##### 2、作用域
JS 有 词法作用域（Lexical Scope）：作用域由代码书写位置决定，而不是调用位置。

###### 三种主要作用域

| 类型                     | 特点                      |
| ---------------------- | ----------------------- |
| **全局作用域**              | 任何地方都能访问，浏览器中是 `window` |
| **函数作用域**              | 每个函数调用创建一个新的作用域         |
| **块级作用域**（`let/const`） | `{}` 内有效，如 if、for       |
###### 作用域链（Scope Chain）
当访问变量时，JS 引擎会按如下顺序查找：

    当前作用域 → 上层作用域 → ... → 全局作用域


例子：

    function a() {
      const x = 1
      function b() {
        const y = 2
        console.log(x, y)  // 能访问 x 是因为作用域链
      }
      b()
    }
    a()

##### 3、闭包（Closure）

闭包 = 函数 + 其父作用域中的变量引用

当一个内部函数在外部被访问时，即使外部函数已经返回，它仍然“记住”父作用域的变量。

例：

    function createCounter() {
      let count = 0
      return function () {
        return ++count
      }
    }
    
    const counter = createCounter()
    console.log(counter()) // 1
    console.log(counter()) // 2


闭包应用：
- ✔ 数据隐藏（私有变量）
- ✔ 工厂函数
- ✔ 函数柯里化
- ✔ 防抖节流

##### 4、JavaScript 对象模型与原型链（Prototype Chain）

JS 的对象基于 原型继承（Prototype Inheritance）。

###### a. 每个对象都有 [[Prototype]]

可以通过：

    Object.getPrototypeOf(obj)

或非标准语法：

    obj.__proto__

###### b. 函数的 prototype

构造函数的每个实例共享 prototype 上的方法：

    function Person(name) {
      this.name = name
    }
    Person.prototype.sayHi = function () {
      console.log("Hi " + this.name)
    }
    
    const p = new Person("Alice")
    p.sayHi()   // 原型链查找 sayHi

###### c. 原型链查找机制

当访问对象属性时：

    对象本身 → 构造函数 prototype → Object.prototype → null

##### 5、this 指向

this 的值 在运行时由调用方式决定。

###### a. 四大核心原则
（1）默认绑定

    function f() { console.log(this) }
    f()  // 浏览器：window；严格模式：undefined

（2）隐式绑定

    const obj = {
      x: 10,
      f() { console.log(this.x) }
    }
    obj.f() // this = obj

（3）显式绑定（call / apply / bind）

    function f() { console.log(this.x) }
    f.call({x: 100}) // 100

（4）new 绑定

    function Person(name) {
      this.name = name
    }
    new Person('Tom') // this = 新对象

###### b. 优先级

    new > 显式绑定 > 隐式绑定 > 默认绑定

##### 6、箭头函数（Arrow Function）

箭头函数是 ES6 引入的一种更简洁的函数写法，但有特殊行为。

###### a. 箭头函数没有自己的 this

它不会创建自己的 this，而是沿用外层作用域的 this（词法 this）。

    const obj = {
      value: 10,
      f: () => {
        console.log(this.value)
      }
    }
    obj.f() // undefined，因为 this = window

###### b. 没有 arguments

用 rest 参数代替：

    const f = (...args) => args

###### c. 不可作为构造函数（没有 new.target）
###### d. 不绑定原型（没有 prototype）
##### 7、生成器函数（Generator Function）

生成器是一类可以 中断执行并恢复 的特殊函数。

###### a. 定义方式

    function* gen() {
      yield 1
      yield 2
      yield 3
    }

###### b. 使用

    const g = gen()
    console.log(g.next()) // {value: 1, done: false}
    console.log(g.next()) // {value: 2, done: false}
    console.log(g.next()) // {value: 3, done: false}
    console.log(g.next()) // {value: undefined, done: true}

###### c. 应用场景

- 异步流程控制（早期替代回调地狱）

- 自定义迭代器（Iterator）

- 无限序列生成（如斐波那契数列）

- 惰性计算（Lazy evaluation）

例：无限自增 generator

    function* infinite() {
      let i = 0
      while (true) {
        yield i++
      }
    }

##### 8、全局总结与知识图谱
###### 作用域 & 闭包

- JS 是 词法作用域

- 闭包是 函数能访问父作用域

- 作用域链控制变量查找

###### 原型链 & 对象

- 所有对象通过 原型链 继承

- 查找属性：对象 → 原型 → 原型的原型 …

###### this 机制

- 根据调用方式动态绑定

- new / call / apply / bind 有最高优先级

###### 箭头函数

- 关键点：没有自己的 this、arguments、prototype、new

###### 生成器函数

- 可以多次暂停与恢复执行

- 精准控制迭代过程


### 2.2 标准库（Standard Library / Built-in Objects）

这是 JavaScript 内置对象和方法集合，提供大量工具和功能，方便开发：

- 对象操作：Object.keys()、Object.values()、Object.entries()、Object.fromEntries()

- 数组操作：Array.prototype.map()、filter()、reduce()、flat()、flatMap()

- 字符串处理：String.prototype.padStart()、padEnd()、replaceAll()

- 异步和Promise：Promise、Promise.allSettled()、fetch()（浏览器标准库）

- 集合与映射：Map、Set、WeakMap、WeakSet

- 日期时间：Date、Intl、Temporal（ES2024 新增）

- 其他工具：Math、JSON、RegExp、console 等

**特点**：标准库提供了常用的工具和接口，但通常需要显式调用，属于语言的“扩展功能”。

**简单比喻**

- 语言核心 = JavaScript 的“骨架和规则”，你写代码的基础

- 标准库 = JavaScript 自带的“工具箱”，提高开发效率

---
## 三、📘 ECMAScript 版本功能汇总

| 主版本               | 发布年份 | 主要功能                                                                                                                                                                             | 分类            |
| ----------------- | ---- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| **ES1**           | 1997 | JavaScript 语言标准化，定义基本语法、数据类型、控制结构等                                                                                                                                               | 语言核心          |
| **ES2**           | 1998 | 编辑性修订，主要是对规范的澄清和修正                                                                                                                                                               | 语言核心          |
| **ES3**           | 1999 | 引入正则表达式、`try/catch`、`switch`、`do-while` 等语法结构                                                                                                                                    | 语言核心          |
| **ES4**           | 未发布  | 原计划引入类、接口、命名空间等特性，但因争议未发布                                                                                                                                                        | -             |
| **ES5**           | 2009 | 严格模式、JSON 支持、`Array.isArray()`、`Object.defineProperty()` 等                                                                                                                       | 语言核心          |
| **ES6 / ES2015**  | 2015 | `let`/`const`、箭头函数、类、模块化（`import/export`）、Promise、生成器、Map/Set、模板字符串、`for...of`、`Symbol`、`Proxy`、`Reflect`、`Intl` 等                                                               | 语言核心          |
| **ES7 / ES2016**  | 2016 | 指数运算符（`**`）、`Array.prototype.includes()`                                                                                                                                         | 语言核心          |
| **ES8 / ES2017**  | 2017 | `async/await`、`Object.entries()`、`Object.values()`、`String.prototype.padStart()`、`String.prototype.padEnd()`、`Object.getOwnPropertyDescriptors()`                                | 语言核心          |
| **ES9 / ES2018**  | 2018 | 异步迭代（`for await...of`）、`Promise.prototype.finally`、正则表达式改进（`s` 标志、命名捕获组）                                                                                                         | 语言核心          |
| **ES10 / ES2019** | 2019 | `Array.prototype.flat()`、`Array.prototype.flatMap()`、`Object.fromEntries()`、`String.prototype.trimStart()`、`String.prototype.trimEnd()`、`Symbol.prototype.description`、`catch` 绑定可选 | 语言核心          |
| **ES11 / ES2020** | 2020 | `BigInt`、`globalThis`、`Promise.allSettled()`、`Nullish Coalescing Operator (??)`、`Optional Chaining (?.)`、`Dynamic Import`、`module` 模式                                            | 语言核心          |
| **ES12 / ES2021** | 2021 | `Logical Assignment Operators (&&=, \|\| =, ??=)`、`Numeric Separators`、`Promise.any()`、`WeakRefs`、`FinalizationRegistry`、`String.prototype.replaceAll()`、`Array.prototype.at()\` | 语言核心          |
| **ES13 / ES2022** | 2022 | 类字段（私有字段、私有方法）、`Top-level await`、`Error.cause`、`Object.hasOwn()`、`Array.prototype.toSorted()`、`Array.prototype.toReversed()`、`Array.prototype.toSpliced()`、`Array.prototype.with()` | 语言核心          |
| **ES14 / ES2023** | 2023 | `Array.prototype.findLast()`、`Array.prototype.findLastIndex()`、`Array.prototype.toSpliced()`、`WeakMap` 支持 `Symbol` 作为键、`Hashbang` 支持、`Array` 和 `TypedArray` 的不可变方法               | 语言核心          |
| **ES15 / ES2024** | 2024 | `Object.groupBy()`、`Map.groupBy()`、`Promise.withResolvers()`、`Set` 的集合操作、正则表达式的 `/v` 标志、`Temporal` API（日期时间处理）                                                                   | 语言核心          |
