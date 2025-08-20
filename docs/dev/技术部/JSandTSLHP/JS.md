# JS
## 1、JS基础
JavaScript 的基础也可以类似 Python 那样分成 语言核心 和 标准库 两部分。

###  语言核心（Language Core）

这是 JavaScript 本身的语法规则和基本行为，是写 JS 代码必须掌握的内容：

- 语法与关键字：if/else、for/while、function、class、async/await、switch 等

- 数据类型：Number、String、Boolean、BigInt、Symbol、null、undefined、对象类型

- 运算符和表达式：算术、逻辑、位运算、空值合并 ??、可选链 ?. 等

- 控制流程与错误处理：try/catch/finally、throw、return

- 函数、闭包、作用域和对象模型：作用域链、原型链、this 指向、箭头函数、生成器函数

**特点**：语言核心决定 JS 代码的基本行为，独立于浏览器或 Node 环境，所有 JS 引擎都必须实现。

###  标准库（Standard Library / Built-in Objects）

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

## 2、📘 ECMAScript 版本功能汇总

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
