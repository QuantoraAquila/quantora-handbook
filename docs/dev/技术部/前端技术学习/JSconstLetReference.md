# JavaScript 中 const 与 let 能声明的所有类型

## 基本数据类型（Primitive Types）

| 类型        | 示例代码                                 |
| --------- | ------------------------------------ |
| Number    | `const age = 25;`                    |
| String    | `let name = "Alice";`                |
| Boolean   | `const isHappy = true;`              |
| null      | `const nothing = null;`              |
| undefined | `let something;`                     |
| Symbol    | `const sym = Symbol('id');`          |
| BigInt    | `const big = 12345678901234567890n;` |

---

## 对象（Object）

```js
const person = {
  name: "Alice",
  age: 30
};

person.age = 31;     // ✅ OK
person = {};         // ❌ 报错，不能重新赋值
```

---

## 数组（Array）

```js
const nums = [1, 2, 3];
nums.push(4);         // ✅ OK
nums = [5, 6];        // ❌ 报错，不能整体重新赋值
```

---

## 函数（Function）

```js
const sayHi = () => {
  console.log("Hi!");
};

const add = function(a, b) {
  return a + b;
};
```

---

## 类（Class）与实例

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

const dog = new Animal("Rex");
```

---

## Promise / async 函数结果

```js
const fetchData = async () => {
  const res = await fetch("https://example.com");
  const data = await res.json();
  return data;
};
```

---

## 模块引用（Import 的结果）

```js
import React from "react";

const App = () => <h1>Hello</h1>;

export default App;
```

---

## 总结表：你可以用 `const` / `let` 定义哪些东西？

| 类型     | 示例代码                              |
| ------ | --------------------------------- |
| 基本数据类型 | `const age = 30;`                 |
| 数组     | `const list = [1,2];`             |
| 对象     | `const obj = {a:1};`              |
| 函数     | `const fn = () => {}`             |
| 类的实例   | `const dog = new Animal();`       |
| 引用值    | `const ref = useRef();` （React 中） |

---

## 使用建议

| 是否修改值？   | 应使用     |
| -------- | ------- |
| ❌ 不会再赋新值 | `const` |
| ✅ 需要变动其值 | `let`   |

> 永远首选 `const`，只有确实需要重新赋值时才用 `let`。
