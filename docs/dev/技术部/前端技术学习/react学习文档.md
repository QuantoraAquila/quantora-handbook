# React 学习笔记（对比原生 JS + JSX + 核心特性）

## 🧠 一、React 的核心特性（相较于原生 JavaScript）

| 特性 | 原生 JS | React |
|------|---------|-------|
| **UI 构建方式** | 通过 DOM API 手动操作 DOM，构建 UI。 | 使用组件构建 UI，自动管理 DOM。 |
| **组件化** | 仅函数封装，无明确组件化支持。 | 完全组件化，易维护和复用。 |
| **状态管理** | 状态和 UI 分离，难以同步。 | `useState`、`useReducer` 等 Hook 驱动 UI 自动更新。 |
| **虚拟 DOM** | 直接操作真实 DOM，性能较低。 | 虚拟 DOM 提升性能，优化更新。 |
| **响应式更新** | 需要手动监听变化并操作 DOM。 | 状态变更自动触发更新。 |
| **事件处理** | 使用 `addEventListener`。 | JSX 中绑定事件如 `onClick={}`。 |
| **模板语法** | HTML 与 JS 分离，字符串拼接繁琐。 | JSX 语法，结构清晰、安全。 |

---

## 💡 二、什么是 JSX？

JSX（JavaScript XML）是 React 推出的 JavaScript 语法扩展，允许在 JS 中编写 HTML 结构。

### 示例对比：

#### 原生 JS 创建元素：
```
js

const element = document.createElement('h1');
element.textContent = 'Hello, World!';
document.body.appendChild(element);
```
#### React+JSX:
```
jsx

const element = <h1>Hello, World!</h1>;
```
## ✨ 三、JSX 的语法特性总结
| 特性         | 说明                                 | 示例                                             |
| ---------- | ---------------------------------- | ---------------------------------------------- |
| 标签写法       | 类似 HTML，但需要闭合                      | `<div />` 或 `<div></div>`                      |
| 表达式插值      | 使用 `{}` 插入 JS 表达式                  | `<p>{name}</p>`                                |
| class 属性   | 要写成 `className`（因为 class 是 JS 保留字） | `<div className="title"></div>`                |
| 事件绑定       | 使用 `onClick={函数名}` 方式              | `<button onClick={handleClick}>Click</button>` |
| 条件渲染       | 使用三元运算符或逻辑 &&                      | `{isLoggedIn ? <Logout /> : <Login />}`        |
| 列表渲染       | 使用 `.map()` 遍历                     | `{items.map(item => <li>{item}</li>)}`         |
| 多个元素必须有根节点 | 必须用一个根元素包裹或使用 `<>...</>`           | `<><h1></h1><p></p></>`                        |

## 🧩 四、React 组件和 JSX 的结合使用
```
jsx

function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const App = () => {
  return (
    <div>
      <Welcome name="Alice" />
      <Welcome name="Bob" />
    </div>
  );
}
```
- 组件像标签一样使用。

- JSX 中可以嵌套组件。

- 数据通过 props 传递。

## 🚀 五、JSX 的优势总结

- ✅ 更直观：HTML结构嵌入 JS 中，更清晰地表达 UI。
- ✅ 更安全：避免字符串拼接造成的 XSS。
- ✅ 更灵活：可嵌入任意 JS 表达式。
- ✅ 更强大：结合组件和状态，构建复杂 UI 更容易。

## 📚 六、React 学习路线图（推荐顺序）
### 1️⃣ 环境搭建
使用 Vite 推荐：

```
bash

npm create vite@latest my-react-app --template react
cd my-react-app
npm install
npm run dev
```
### 2️⃣ 函数组件 vs 类组件
函数组件（推荐）：
```
jsx

function Hello(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

类组件（了解即可）：
``` 
jsx

class Hello extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### 3️⃣ React Hooks（核心技能）
| Hook                      | 用途            |
| ------------------------- | ------------- |
| `useState`                | 管理组件内状态       |
| `useEffect`               | 副作用处理（生命周期）   |
| `useRef`                  | 获取 DOM / 缓存变量 |
| `useContext`              | 跨组件共享状态       |
| `useReducer`              | 更复杂的状态逻辑      |
| `useMemo` / `useCallback` | 性能优化          |

#### 示例：useState + useEffect
```
jsx

import { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `你点击了 ${count} 次`;
  }, [count]);

  return (
    <div>
      <p>当前计数：{count}</p>
      <button onClick={() => setCount(count + 1)}>增加</button>
    </div>
  );
}
```
#### 4️⃣ 组件间通信
| 类型  | 方法            | 示例                        |
| --- | ------------- | ------------------------- |
| 父传子 | props         | `<Child name="Tom" />`    |
| 子传父 | 回调函数          | `onChange={handleChange}` |
| 兄弟间 | 状态提升或 context | `useContext`              |

#### 5️⃣ 条件与列表渲染
##### 条件渲染：
```
jsx

{isLoggedIn ? <Dashboard /> : <Login />}
```
##### 列表渲染：
```
jsx

<ul>
  {items.map(item => <li key={item.id}>{item.name}</li>)}
</ul>
```
#### 6️⃣ 事件处理
```
jsx

function handleClick() {
  alert('按钮被点击了');
}

<button onClick={handleClick}>点击我</button>
```
#### 7️⃣ 表单处理
```
jsx

const [value, setValue] = useState('');

<input
  type="text"
  value={value}
  onChange={(e) => setValue(e.target.value)}
/>
```
#### 8️⃣ React Router（页面跳转）
##### 安装：
```
bash

npm install react-router-dom
```
##### 示例：
```
jsx

import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">首页</Link>
        <Link to="/about">关于</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```
#### 9️⃣ 状态管理方案（进阶）
| 工具             | 特点           |
| -------------- | ------------ |
| Context API    | 简单项目足够用      |
| Redux          | 老牌复杂项目状态管理方案 |
| Zustand        | 现代、简洁、轻量     |
| Recoil / Jotai | 函数式友好，新兴     |

