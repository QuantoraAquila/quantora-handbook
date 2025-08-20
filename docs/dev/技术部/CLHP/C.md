# C
## 1、C基础
### 🔹 1. C 语言核心（Language Core）

C 的核心部分主要是 语法规则、关键字、类型系统和内存模型，这些是编译器必须实现的，不依赖库。

**📌 主要组成：**

- 基本数据类型

    - 整数类型：`char`、`short`、`int`、`long`、`long long`

    - 浮点类型：`float`、`double`、`long double`

    - 特殊类型：`void`、`_Bool`、`_Complex`（C99 引入）

- 类型修饰符

    - `signed`、`unsigned`、`const`、`volatile`、`restrict`

- 控制结构

    - 条件分支：`if`、`else`、`switch`

    - 循环：`for`、`while`、`do...while`

    - 跳转：`break`、`continue`、`goto`、`return`

- 函数与作用域

    - 函数定义、函数声明（原型）

    - 作用域：块作用域、文件作用域、函数作用域

    - 存储类别：`auto`、`register`、`static`、`extern`

- 指针与数组

    - 指针运算、数组与指针关系

    - 动态内存访问

- 结构化数据

    - `struct`、`union`、`enum`

    - 位域（bit field）

- 运算符

    - 算术运算符：`+ - * / %`

    - 逻辑运算符：`&& || !`

    - 位运算符：`& | ^ ~ << >>`

    - 条件运算符：`?:`

    - 赋值运算符：`= += -= *= ...`

### 🔹 2. C 标准库（Standard Library）

C 标准库是一组 头文件 + 库函数，用于提供常用功能，避免程序员重复造轮子。

**📌 常见头文件与功能：**

- 输入输出

    - `<stdio.h>`：标准 I/O（`printf、scanf、fopen、fgets`）

- 通用工具

    - `<stdlib.h>`：内存分配（`malloc、free`）、随机数（`rand`）、进程控制（`exit`）

    - `<assert.h>`：运行时断言

    - `<errno.h>`：错误码定义

- 字符串与内存操作

    - `<string.h>`：字符串操作（`strlen、strcpy、strcat、strcmp`）

    - `<stddef.h>`：常用宏（`size_t、NULL、offsetof`）

- 数学计算

    - `<math.h>`：数学函数（`sin、cos、pow、sqrt`）

    - `<complex.h>`（C99）：复数运算

    - `<tgmath.h>`（C99）：泛型数学宏

- 时间和日期

    - `<time.h>`：时间操作（`time、clock、strftime`）

- 本地化与国际化

    - `<locale.h>`：本地化设置

    - `<wchar.h>、<wctype.h>`（C95）：宽字符和多字节字符支持

    - `<uchar.h>`（C11）：UTF-16、UTF-32

- 并发与原子操作

    - `<threads.h>`（C11）：线程支持（`thrd_t、mtx_t、cnd_t`）

    - `<stdatomic.h>`（C11）：原子操作

- 位操作（最新）

    - `<stdbit.h>`（C23）：位操作工具函数

## 2、📘 C 语言标准版本改动汇总表
| 版本               | 年份        | 语言核心改动                                                                    | 标准库改动                                                      |
| ---------------- | --------- | ------------------------------------------------------------------------- | ---------------------------------------------------------- |
| **K\&R C**       | 1978/1988 | 基础类型（`int`、`char`、`float`）、指针、数组、函数、控制结构（`if`/`for`/`while`/`switch`）、结构体 | 无统一标准库（编译器厂商自带）                                            |
| **C89 / ANSI C** | 1989      | 函数原型、`const`、`volatile`、`enum`                                            | 标准库首次规范：`<stdio.h>`、`<stdlib.h>`、`<string.h>`、`<math.h>` 等 |
| **C90 / ISO C**  | 1990      | 与 C89 等价                                                                  | 与 C89 等价                                                   |
| **C95**          | 1995      | 宽字符与多字节字符支持                                                               | 新增 `<wchar.h>`、`<wctype.h>`                                |
| **C99**          | 1999      | `inline`、可变长数组（VLA）、复合字面量、单行注释 `//`、新类型：`long long`、`_Bool`、`_Complex`    | `<stdint.h>`（固定宽度整数）、`<tgmath.h>`、新数学函数（如 `fmax`、`round`）  |
| **C11**          | 2011      | `_Noreturn`、`_Alignas`、`_Static_assert`、多线程内存模型                           | `<threads.h>`（标准线程 API）、`<uchar.h>`（UTF-16/UTF-32）、安全函数族   |
| **C17**          | 2017      | 语法无重大变化（C11 修订）                                                           | 与 C11 基本一致                                                 |
| **C23**          | 2023      | `nullptr`、`typeof`、增强的属性语法、改进的 `static_assert`                            | `<stdbit.h>`（位操作工具）、Unicode 与标准函数增强                        |
**📌 总结**

- C89/C90 → 第一次标准化，确立语言和标准库的基本框架。

- C99 → 最大革新（新类型、新语法、<stdint.h>）。

- C11 → 并发与 Unicode 支持。

- C17 → 修订版，无实质新特性。

- C23 → 现代化改进，部分与 C++ 接轨。