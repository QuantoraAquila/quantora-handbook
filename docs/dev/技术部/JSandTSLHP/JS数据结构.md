# JS数据类型
## 零、JS底层存储机制

### 0.1 stack and heap：

| 存储区          | 内容                 | 示例           |
| ------------ | ------------------ | ------------ |
| **栈（Stack）** | 存放变量标识符（名称）和基本类型的值 | `let a = 10` |
| **堆（Heap）**  | 存放复杂数据（引用类型的实际内容）  | 对象、数组、函数等    |
举个例子：

    const user = { name: 'Tom' };


内存结构如下：
    
    栈内存：
    ┌──────────────┐
    │ user ─┐      │
    └──────────────┘
               ↓
    堆内存：
    ┌───────────────────────────┐
    │ { name: 'Tom' }           │
    └───────────────────────────┘

user 变量在栈(高速缓存区)中存储的是一个“指针”（引用地址），指向堆（低速缓存区）中的对象。

#### 0.1.1 从上到下的存储层级（CPU → 硬盘）

| 层级   | 名称      | 英文                | 速度       | 容量         | 作用              |
| ---- | ------- | ----------------- | -------- | ---------- | --------------- |
| 🧩 1 | 寄存器     | **CPU Registers** | 极快（纳秒级）  | 极小（几十个字节）  | 存放CPU正在执行的指令和数据 |
| ⚡ 2  | 一级缓存    | **L1 Cache**      | 极快       | 几十KB       | CPU 内部缓存        |
| ⚡ 3  | 二级缓存    | **L2 Cache**      | 很快       | 数百KB ~ 几MB | CPU 内部或近旁       |
| ⚡ 4  | 三级缓存    | **L3 Cache**      | 快        | 数MB ~ 数十MB | 多核共享缓存          |
| 💾 5 | 主存      | **RAM / 内存条**     | 中速（几十纳秒） | GB级        | 存放正在运行的程序和数据    |
| 🧱 6 | 磁盘      | **SSD/HDD**       | 慢（微秒~毫秒） | TB级        | 长期存储文件          |
| ☁️ 7 | 网络 / 云端 | Network           | 极慢       | 无限         | 远程存储            |

#### 0.1.2 “栈”和“堆”到底在这层结构里属于哪里？
✅ 栈（Stack）和堆（Heap）都存在于内存（RAM）中。

它们不是“物理位置不同”，而是：

一种逻辑上的内存管理方式。

| 名称       | 所在物理位置   | 管理方式            | 分配特性        |
| -------- | -------- | --------------- | ----------- |
| 栈（Stack） | RAM 中的栈区 | 由编译器自动分配/释放     | 后进先出（LIFO）  |
| 堆（Heap）  | RAM 中的堆区 | 由程序员（或JS引擎）动态分配 | 任意分配/释放（无序） |
#### 0.1.3 JS 执行时的内存布局（简化示意）

在浏览器或 Node.js 运行 JS 时，JS 引擎（V8 等）会为当前线程分配内存：

    ┌────────────────────┐
    │   栈 Stack (RAM中) │ ← 存储局部变量、函数调用记录、原始值
    │────────────────────│
    │   堆 Heap (RAM中)  │ ← 存储对象、数组、函数等引用类型
    │────────────────────│
    │   代码区 Code      │ ← 存放 JS 编译后的字节码
    └────────────────────┘


例如：

    const user = { name: 'Tom' };
    let age = 20;


在内存中的分布是：

| 区域 | 内容                               |
| -- | -------------------------------- |
| 栈  | `user → 指针0x1000`, `age → 20`    |
| 堆  | 地址 `0x1000` 存放 `{ name: 'Tom' }` |
#### 0.1.4 CPU 执行时访问路径

当 JS 引擎执行代码时，访问变量的路径如下：

- CPU 通过寄存器取到变量地址

- 如果在 L1/L2 Cache 中有缓存命中，直接读取 ✅

- 若没有命中，访问主存（RAM）

- 如果内存不在 RAM（例如程序未加载完），操作系统会从 SSD 读入到内存（分页机制）

👉 所以：

- 栈数据访问更快：在内存中连续分布，局部性高，容易被 CPU 缓存命中。

- 堆数据访问稍慢：分布不连续，需要间接寻址（通过指针访问）。

#### 0.1.5 为什么说“栈更靠近高速缓存”

虽然“栈”和“堆”都在 RAM 中，
但在访问特性上：

| 项目    | 栈 (Stack) | 堆 (Heap) |
| ----- | --------- | -------- |
| 分配方式  | 编译器自动     | 程序运行时动态  |
| 内存布局  | 连续        | 离散       |
| 生命周期  | 函数调用结束即释放 | 手动或GC释放  |
| 缓存命中率 | 高（局部性好）   | 低（随机分布）  |
| 访问速度  | 更快        | 稍慢       |
因此人们常说：

- “栈数据通常在 CPU 高速缓存中访问，而堆数据常在主内存中访问。”

但要明确：

- 它们的物理位置都在内存条（RAM）里。

- CPU cache 是对 RAM 内容的高速复制，不是单独放“栈”。

#### 0.1.6 延伸：硬盘什么时候参与？

硬盘（SSD/HDD）只有在以下情况才被访问：

- 程序加载时（JS 文件从硬盘 → 内存）

- 操作系统分页（RAM 不够时将部分内存换出到磁盘）

- 数据持久化（例如 localStorage、数据库、文件 I/O）

在 JS 运行时，所有变量（无论栈或堆）都在 RAM 中，与硬盘无关。

#### 0.1.7 总结精华

| 层级        | 存储介质     | 内容        | 管理者        | 特点         |
| --------- | -------- | --------- | ---------- | ---------- |
| CPU Cache | CPU 内部缓存 | 热数据       | 硬件自动管理     | 极快、容量小     |
| 栈 Stack   | RAM 中    | 局部变量、函数调用 | 编译器        | 连续、快速、自动释放 |
| 堆 Heap    | RAM 中    | 对象、数组、函数  | JS 引擎 (GC) | 离散、灵活、需回收  |
| 硬盘 Disk   | SSD/HDD  | 程序文件、数据   | 操作系统       | 持久存储、慢     |


## 0.2 理解 `const` 的本质约束
👉 `const` 的本质是：

- 变量 绑定的引用（指针）不可被重新赋值。

也就是说：

- 不能改“指针”本身
- 但可以改“指针指向的内容”

对应到内存层面：

| 操作         | 是否修改引用地址   | 是否允许 |
| ---------- | ---------- | ---- |
| 修改对象属性     | ❌ 否，只改堆中数据 | ✅ 允许 |
| 重新赋值给另一个对象 | ✅ 是，改了引用地址 | ❌ 禁止 |

示例：

    const obj = { x: 1 };
    
    // ✅ 改堆中数据（引用不变）
    obj.x = 2;
    
    // ❌ 改引用（栈中的指针）
    obj = { x: 3 }; // TypeError

## 0.3 let 与 const 的区别

| 特性        | `let`    | `const`       |
| --------- | -------- | ------------- |
| 是否可重新赋值   | ✅ 是      | ❌ 否           |
| 是否可修改内部属性 | ✅ 是      | ✅ 是（引用不变即可）   |
| 初始化是否必须赋值 | ❌ 否      | ✅ 是           |
| 常见用途      | 可能被更新的变量 | 不会被重新绑定的对象或函数 |

## 0.4 数组与对象的示例对比
✅ const 不可重绑定，但可改内容

    const arr = [1, 2];
    arr.push(3); // ✅
    console.log(arr); // [1,2,3]
    
    arr = [4,5]; // ❌ 报错

✅ let 可重绑定

    let arr = [1, 2];
    arr = [3, 4]; // ✅


总结一句话：

`const` 锁定的是“引用地址”，不是“堆内存内容”。

## 0.5 可视化理解

假设你声明：

    const obj = {a: 1};
    
    栈（Stack）                 堆（Heap）
    ┌─────────────┐      ┌────────────────────┐
    │ obj ─┐       │──→  │ { a: 1 }           │
    └─────────────┘      └────────────────────┘
    

执行：

    obj.a = 2;


此时只是在堆中修改 { a: 1 } → { a: 2 }，
栈中的指针没变，const 约束没有被破坏。

执行：

    obj = { a: 3 };


这会让 obj 尝试指向一个新的堆地址，
违反 const 的约束 → ❌ 报错。

## 0.6 为什么推荐对引用类型使用 const？

在现代 JS 编码规范中（例如 Airbnb ESLint、Google JS Style），
推荐：

- 默认使用 const
- 仅当你需要重新绑定变量时，才使用 let

原因：

- 防止意外覆盖引用

        const users = [];
        // 不会意外写成 users = {} 导致 bug


- 保证数据引用稳定  
  
  这样传入函数、事件回调时，指针地址不会意外变。

- 更易于内存优化

  引用稳定有助于垃圾回收（GC）判断变量生命周期。

## 0.7 深入一点：堆中对象的共享引用

    const a = { value: 1 };
    const b = a;
    
    b.value = 2;
    console.log(a.value); // 2（同一引用）


图示：

    栈内存：
    a ─┐
    b ─┘────→ 堆内存 { value: 2 }


即使 `a` 和 `b` 都是 `const`，
它们共享同一个堆内对象，因此修改内容会互相影响。

## 0.8 与垃圾回收的关系（GC 简述）

当引用类型的 最后一个引用变量 被销毁（栈中指针删除）时，
堆内的数据会被 垃圾回收机制（Garbage Collector） 自动清除。

    let obj = { a: 1 };
    obj = null; // 原 {a:1} 的堆内对象已无引用，等待 GC 清理

`const` 不会阻止垃圾回收，只是阻止重新赋值。

## 0.9 总结精华

| 概念                               | 描述               |
| -------------------------------- | ---------------- |
| **`const` 锁定的是引用地址，不是堆内内容**      | ✅ 改属性可以，改引用不行    |
| **引用类型存在堆中，变量在栈中保存地址指针**         | 所以多个变量可能共享同一对象   |
| **修改内容不违反 const 约束**             | 因为栈中引用没变         |
| **`let` 用于会变的绑定，`const` 用于稳定引用** | 推荐默认使用 const     |
| **GC 回收只看是否还有引用**                | 不会因为 const 而阻止清理 |

## 🧠 一、引用类型基础：Object（对象）
### 1.1 创建对象的三种方式

    // ① 字面量
    const person = { name: 'Tom', age: 25 };
    
    // ② 构造函数
    const user = new Object();
    user.name = 'Alice';
    user.age = 30;
    
    // ③ 工厂函数 / 类
    function createPerson(name, age) {
    return { name, age };
    }

### 1.2 访问与修改

    console.log(person.name);    // 点操作符
    console.log(person['age']);  // 方括号
    person.city = 'Beijing';     // 新增属性
    delete person.age;           // 删除属性

### 1.3 遍历对象

    for (let key in person) {
      console.log(key, person[key]);
    }
    
    Object.keys(person);   // ['name', 'city']
    Object.values(person); // ['Tom', 'Beijing']
    Object.entries(person); // [['name','Tom'], ['city','Beijing']]

### 1.4 对象的拷贝
#### 1.4.1 浅拷贝（只复制第一层）

    const obj1 = { a:1, b:{c:2} };
    const obj2 = {...obj1};
    obj2.b.c = 99;
    console.log(obj1.b.c); // ❗99（引用共享）

#### 1.4.2 深拷贝（递归复制）

    const deepClone = JSON.parse(JSON.stringify(obj1));
    deepClone.b.c = 88;
    console.log(obj1.b.c); // ✅2（互不影响）


⚠️ 注意：JSON 方法无法复制函数、undefined、Symbol 等。

**更安全的深拷贝可以用：**
- structuredClone(obj)（原生支持）
- 或手写递归函数。

## 🧮 二、Array（数组）
### 2.1 创建数组

    const arr1 = [1, 2, 3];
    const arr2 = new Array(3).fill(0); // [0, 0, 0]
### 2.2 常用方法分类

| 分类   | 方法                                                    | 示例 |
| ---- | ----------------------------------------------------- | -- |
| 增删   | push(), pop(), shift(), unshift(), splice()           |    |
| 连接   | concat(), join(), spread(...)                         |    |
| 查找   | indexOf(), find(), includes()                         |    |
| 遍历   | forEach(), map(), filter(), reduce(), some(), every() |    |
| 排序   | sort(), reverse()                                     |    |
| 复制切片 | slice()                                               |    |
**示例：**

    let arr = [1,2,3,4,5];
    
    arr.push(6);          // [1,2,3,4,5,6]
    arr.pop();            // [1,2,3,4,5]
    arr.slice(1,4);       // [2,3,4]
    arr.splice(2,1);      // 删除索引2
    arr.map(x => x*2);    // [2,4,6,8,10]

### 2.3 多维数组

    const matrix = [
      [1, 2],
      [3, 4]
    ];
    console.log(matrix[1][0]); // 3

### 2.4 数组去重

    const unique = [...new Set([1,2,2,3,3])]; // [1,2,3]

## ⚙️ 三、Function（函数）也是对象

### 3.1 在 JS 中，函数本质上是对象，有属性和方法。

    function greet(name) {
      console.log('Hi, ' + name);
    }
    greet.language = 'English'; // 给函数加属性

### 3.2 高阶函数（函数作为参数或返回值）

    function doTwice(fn) {
      fn();
      fn();
    }
    doTwice(() => console.log('Hello'));

### 3.3 闭包（Closure）

闭包使函数可以访问外层作用域的变量：

    function counter() {
      let count = 0;
      return function() {
        count++;
        return count;
      };
    }
    const c = counter();
    console.log(c()); // 1
    console.log(c()); // 2

## 📅 四、Date（日期对象）

    const now = new Date();
    console.log(now.toISOString()); // 2025-11-13T08:00:00.000Z
    
    const d = new Date('2025-11-01');
    d.getFullYear();  // 2025
    d.getMonth();     // 10（0开始计）
    d.getDate();      // 1
    d.getTime();      // 时间戳

## 🧭 五、Map 与 Set（ES6 引入）
### 5.1 Map：键值对集合（比 Object 更灵活）

    const map = new Map();
    map.set('name', 'Tom');
    map.set('age', 25);
    console.log(map.get('name')); // Tom
    console.log(map.size);        // 2
    map.delete('age');


**✅ 特点：**
- 键可以是任意类型（对象、函数、原始值）
- 有序
- 可迭代


    for (let [k, v] of map) {
      console.log(k, v);
    }

### 5.2 Set：无重复值的集合

    const set = new Set([1, 2, 2, 3]);
    set.add(4);
    set.delete(2);
    console.log(set.has(3));  // true
    console.log([...set]);    // [1,3,4]


**常见用途：**
- 数组去重
- 求交集/并集/差集


    const a = new Set([1,2,3]);
    const b = new Set([3,4,5]);
    
    // 并集
    const union = new Set([...a, ...b]);
    // 交集
    const intersect = new Set([...a].filter(x => b.has(x)));
    // 差集
    const diff = new Set([...a].filter(x => !b.has(x)));

## 🧩 六、WeakMap 与 WeakSet

这些是 弱引用结构（不会阻止垃圾回收）。

| 特性   | WeakMap | WeakSet |
| ---- | ------- | ------- |
| 键类型  | 只能是对象   | 只能是对象   |
| 可遍历  | ❌ 否     | ❌ 否     |
| 垃圾回收 | ✅ 自动清理  | ✅ 自动清理  |

用途：缓存、存储临时关联数据。

    const wm = new WeakMap();
    let obj = {};
    wm.set(obj, 'data');
    obj = null; // 对象被销毁后，WeakMap 自动清除引用

## 🧱 七、复杂数据结构综合示例
示例：记录每个用户的交易历史

    const users = new Map();
    
    function addTransaction(user, amount) {
      if (!users.has(user)) {
        users.set(user, []);
      }
      users.get(user).push({
        amount,
        time: new Date().toISOString()
      });
    }
    
    addTransaction('Alice', 200);
    addTransaction('Alice', -50);
    addTransaction('Bob', 300);
    
    console.log(users.get('Alice'));


输出：

    [
      { amount: 200, time: '2025-11-13T08:30:00.000Z' },
      { amount: -50, time: '2025-11-13T09:00:00.000Z' }
    ]

## 🧠 八、总结对比表

| 类型      | 是否可迭代 | 是否可重复 | 键类型限制  | 应用场景    |
| ------- | ----- | ----- | ------ | ------- |
| Object  | ❌     | ✔️    | 字符串/符号 | 一般数据结构  |
| Array   | ✔️    | ✔️    | 索引（数字） | 列表/队列   |
| Map     | ✔️    | ✔️    | 任意类型   | 快速查找映射  |
| Set     | ✔️    | ❌     | 任意类型   | 去重/集合运算 |
| WeakMap | ❌     | ✔️    | 仅对象    | 缓存、私有数据 |
| WeakSet | ❌     | ❌     | 仅对象    | 引用追踪    |
