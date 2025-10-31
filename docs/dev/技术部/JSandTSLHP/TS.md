# TS
## 1、TS基础
### 语言核心（Language Core）

这是 TypeScript 本身的语法规则和语言特性，决定你写 TS 代码的基本行为：

- 静态类型系统：类型注解 (: string)、类型推断、接口 (interface)、类型别名 (type)、联合类型、交叉类型、泛型、never、unknown 等

- 类和面向对象：类 (class)、继承 (extends)、访问修饰符 (public、private、protected)、抽象类、接口实现

- 函数与高级类型：函数类型、可选参数、默认参数、重载、this 类型

- 控制流程与语法增强：for...of、async/await、装饰器、条件类型、映射类型、模板字面量类型

- 与 JavaScript 的兼容：TS 是 JS 的超集，所有 JS 语法都是 TS 核心的一部分

**特点**：语言核心决定 TS 代码的类型检查、编译行为和语法规则。

### 标准库（Standard Library / Built-in Types）

TypeScript 的标准库主要是 类型声明文件（.d.ts），提供对 JavaScript 内置对象、DOM、Node.js API 等的类型支持：

- JavaScript 内置对象类型：Array<T>、String、Number、Boolean、Object、Map、Set、Promise 等

- DOM 类型库（浏览器环境）：Document、HTMLElement、Event、NodeList 等

- Node.js 类型库（Node 环境）：Buffer、fs、http 等

- 辅助类型：Partial<T>、Required<T>、Readonly<T>、Record<K,T>、Pick<T,K>、Omit<T,K> 等

**特点**：标准库提供了类型信息和工具类型，方便在 TS 中安全使用 JS API，但通常需要 import 或通过 @types 获取额外声明。

**简单比喻**

- 语言核心 = TypeScript 的“骨架和类型系统”，控制语法和类型检查

- 标准库 = TypeScript 的“类型工具箱”，提供对 JS 内置对象、DOM、Node 等的类型支持

---
## 2、TS版本变动、

| 主版本        | 发布年份    | 主要功能                                                    | 分类   |
| ---------- | ------- | ------------------------------------------------------- | ---- |
| **TS 1.0** | 2012    | TypeScript 初始版本，提供静态类型检查，支持 ES3 和 ES5 特性                | 语言核心 |
| **TS 1.4** | 2013    | 引入泛型、模块、命名空间、接口、类等面向对象特性                                | 语言核心 |
| **TS 1.5** | 2015    | 支持 ES6 模块、`let` 和 `const` 声明、模板字符串、类型别名等                | 语言核心 |
| **TS 1.6** | 2015    | 引入 `async` 和 `await`，支持 Promise 类型                      | 语言核心 |
| **TS 1.8** | 2016    | 支持 JSX，增强类型推断，提供类型守卫等                                   | 语言核心 |
| **TS 2.0** | 2016    | 引入严格模式，支持 `null` 和 `undefined` 类型，增强类型推断                | 语言核心 |
| **TS 2.1** | 2016    | 引入条件类型、映射类型、`keyof` 操作符等高级类型特性                          | 语言核心 |
| **TS 2.7** | 2018    | 引入 `const` 命名属性、固定长度元组等特性                               | 语言核心 |
| **TS 3.0** | 2018    | 引入元组作为 REST 参数、生成器、`unknown` 类型、JSX 中的 `defaultProps` 等 | 语言核心 |
| **TS 3.7** | 2019    | 引入可选链操作符 `?.`、空值合并操作符 `??`、断言函数等                        | 语言核心 |
| **TS 4.0** | 2020    | 引入变参元组、命名元组元素、更好的 JSX 支持等                               | 语言核心 |
| **TS 4.4** | 2021    | 引入 `satisfies` 操作符、类的自动访问器等特性                           | 语言核心 |
| **TS 5.0** | 2023    | 引入新的装饰器标准、改进的泛型推断、增强的 JSDoc 功能等                         | 语言核心 |
| **TS 5.8** | 2025    | 引入条件返回类型的更细粒度检查等                                        | 语言核心 |
| **TS 6.0** | 预计 2025 | 作为过渡版本，为 TypeScript 7.0 做准备，可能引入弃用某些设置和更新类型检查行为等        | 语言核心 |

### 📚 分类说明

- 语言核心（Language Core）：包括语法、关键字、数据类型、控制结构等，是 TypeScript 语言的基础部分，所有 TypeScript 引擎都必须实现。

- 标准库（Standard Library）：包括内置对象和方法，如 Array、Object、String、Promise 等，是 TypeScript 语言的标准库，提供了丰富的功能支持。

## TypeScript（TS）的大部分版本变动确实都算在语言核心，原因主要有以下几点：

### 1. TypeScript 是 JavaScript 的超集

- TS 的目标就是在 JavaScript 的语言核心上加上类型系统和语法扩展。

- 它并没有像 Python/C 一样自带庞大的 标准库。

- TS 程序运行时依赖的是 JS 的标准库（ECMAScript + DOM API + Node.js API 等），所以它自身不需要维护独立的“标准库层”。

### 2. 变动主要是语法和类型系统

TS 的每个大版本更新，几乎都是：

- 类型系统增强：

    - 新的类型运算符（infer、keyof、satisfies）

    - 严格模式改进（strictNullChecks、exactOptionalPropertyTypes）

    - 模板字面量类型（Template Literal Types）

- 语法兼容增强：

    - 跟进 ECMAScript 新语法（如 ES6 的 async/await、ES2020 的可选链 ?.）

    - 提供类型推导/检查机制

- 工具性特性：

    - 装饰器（Decorators）

    - 模块系统（import/export 的类型支持）

    - 这些都属于 语言核心 的扩展，而不是库函数。

### 3. “标准库”由声明文件定义

TS 没有独立的运行时库，所谓的“库”其实就是一堆 **类型声明文件（.d.ts）**：

- lib.d.ts → 对应 ECMAScript 标准库（如 Array, Map, Promise）

- dom.d.ts → 浏览器环境 API（如 document, HTMLElement）

- node.d.ts（第三方提供） → Node.js API

这些 .d.ts 文件只是在编译时提供类型检查，不会影响运行时。也就是说：

- TS 标准库的更新几乎等于 JS 标准库的更新，所以 TS 自己的“库改动”非常少。

- 真正的变动还是体现在 语言核心（类型系统 + 语法支持）。