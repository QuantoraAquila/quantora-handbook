# JavaScript 与 TypeScript 数据类型全对比（含简写形式）

## 🧩 一、JS 与 TS 共有的基础类型

| 类型英文名 | TS 简写类型             | 示例值                | 用途与说明                     |
|------------|--------------------------|------------------------|--------------------------------|
| Number     | `number`                 | `1`, `3.14`, `-100`    | 所有数字（整数、浮点数）      |
| String     | `string`                 | `"hello"`, `'abc'`     | 文本、字符序列                 |
| Boolean    | `boolean`                | `true`, `false`        | 布尔值                         |
| Null       | `null`                   | `null`                 | 主动清空变量、空值             |
| Undefined  | `undefined`              | `undefined`            | 默认未赋值、函数无返回         |
| Object     | `object`                 | `{}`, `{a: 1}`          | 通用对象类型                   |
| Array      | `type[]` / `Array<type>` | `[1, 2]`, `["a", "b"]`  | 有序集合（JS 实为对象）       |
| Function   | `() => void` / `Function`| `() => {}`             | 可调用的对象                   |
| Symbol     | `symbol`                 | `Symbol("id")`         | 创建唯一标识符（ES6）          |
| BigInt     | `bigint`                 | `123n`, `BigInt(10)`   | 超大整数（ES2020+）            |

---

## 🔹 二、TypeScript 独有的类型与写法（静态增强类型）

| 类型英文名     | TS 类型写法（简写）         | 示例                          | 说明                           |
|----------------|-----------------------------|-------------------------------|--------------------------------|
| 任意类型       | `any`                        | `let x: any = "abc"`         | 跳过类型检查（谨慎使用）       |
| 未知类型       | `unknown`                    | `let x: unknown = value`     | 安全的 `any`，使用前需判断     |
| 无返回值函数   | `void`                       | `function log(): void {}`    | 表示函数无返回                 |
| 永不返回       | `never`                      | `throw new Error()`          | 错误、死循环                   |
| 元组           | `[type1, type2, ...]`        | `[number, string]`           | 固定结构的数组                 |
| 联合类型       | `A \| B`                      | `string \| number`            | 可为多个类型之一               |
| 交叉类型       | `A & B`                      | `{a: string} & {b: number}`  | 类型合并                       |
| 类型别名       | `type Name = ...`            | `type ID = string | number`  | 自定义类型名                   |
| 接口           | `interface`                  | `interface User { id: number }` | 描述对象结构               |
| 枚举           | `enum`                       | `enum Color {Red, Blue}`     | 常量集合                       |
| 类型断言       | `as type` / `<type>`         | `value as string`            | 强制指定类型                   |
| 泛型           | `<T>`                        | `Array<T>`, `function<T>()`  | 类型参数                       |
| 可选属性       | `?`                          | `age?: number`               | 可选的字段或参数               |
| 只读属性       | `readonly`                   | `readonly name: string`      | 不可修改属性                   |
| 字面量类型     | 字面值                       | `"left"`, `42`               | 只允许特定值                   |
| 空值合并       | `??`                         | `x = a ?? b`                 | null/undefined 则取默认值      |
| 可选链         | `?.`                         | `user?.name`                 | 安全访问属性                   |

---

## 🔹 三、常见对象类型写法简表（TS 简写风格）

| 类型                 | 简写形式（TS）                  | 示例                                      |
|----------------------|---------------------------------|-------------------------------------------|
| 数组（统一类型）     | `number[]`                      | `let arr: number[] = [1, 2, 3]`           |
| 数组（泛型写法）     | `Array<string>`                 | `let arr: Array<string> = ["a", "b"]`     |
| 对象结构             | `{ key: type }`                 | `let obj: { name: string }`              |
| 元组（固定结构）     | `[number, string]`              | `let t: [number, string] = [1, "a"]`     |
| 函数类型（简写）     | `() => returnType`              | `let f: () => void`                      |
| 联合类型             | `type1 \| type2`                 | `string \| number`                         |
| 交叉类型             | `type1 & type2`                 | `{id: number} & {name: string}`          |
| 可选字段             | `key?: type`                    | `{ age?: number }`                       |
| 只读字段             | `readonly key: type`            | `{ readonly id: number }`                |

---

## 🧠 四、类型分类图

```text
  JavaScript 原始类型（TS 也支持）：
  ┌────────────┐
  │ string     │
  │ number     │
  │ boolean    │
  │ null       │
  │ undefined  │
  │ symbol     │
  │ bigint     │
  └────────────┘

  引用类型（JS / TS）：
  ┌────────────┐
  │ object     │
  │ array      │
  │ function   │
  └────────────┘

  TypeScript 增强类型（仅 TS）：
  ┌─────────────────────────────────────┐
  │ any, unknown, void, never           │
  │ tuple, enum, literal type           │
  │ union（|）, intersection（&）        │
  │ type alias, interface, generics     │
  │ optional（?）, readonly              │
  │ type assertion（as）                │
  └─────────────────────────────────────┘
