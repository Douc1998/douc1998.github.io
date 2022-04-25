---
title: React Hooks 入门
date: 2022-04-23 14:43:07
tags: 
    - 前端
    - React
categories: React  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo/react.png  # 文章页头部图片
cover: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo/react.png  # 主页中显示的文章封面图片
keywords: "React, Hooks"
---
# 前言
[React](https://zh-hans.reactjs.org/) 作为前端三大主流框架之一，得到了许多开发者的亲睐。其**组件化**编程模式大大简化了前端开发过程，可以更好地管理组件间的逻辑和依赖关系。

刚开始在组里做项目的时候，好不容易掌握了 React 的一些基础知识，比如组件化、DOM渲染、生命周期等等，心里沾沾自喜觉得自己也算是入门了。结果，师兄突然跟我说：“你快去把 [React Hooks](https://zh-hans.reactjs.org/docs/hooks-faq.html#which-versions-of-react-include-hooks) 搞明白，这样开发起来就更方便了。” 于是我一个前端小白又开始去网络上搜索各种教程以及开发文档学习`React Hooks`。

后来因为一些琐事忘记做学习笔记了，不过经过这段时间的实践和学习，现在也算是对 `React Hooks` 有了一点自己的理解。下面我将简单谈一谈 **React Hooks的入门内容**。

# 什么是 Hooks ？
`Hooks` 的中文解释为 “**钩子**”，而 `React Hooks` 则是在 v16.8 版本发布的一种全新的API，可以说是颠覆了 React 一直以来的**类组件**的写法模式。

## 类组件
写过 React 的人都知道，我们刚入门所接触的 React 都是从类组件开始，如下：
```jsx
class HelloWorld extends React.Component {
    render() {
        return <div>Hello World !</div>;
    }
}
```
从上面看，写组件类的时候好像不需要太多的代码量。但实际上，当我们的组件状态增多，业务逻辑变复杂时，组件类将会变得十分复杂（写项目的时候会有明显感觉）。

组件类便于我们理解组件内部逻辑，管理组件各种状态，但也存在一些缺点。Redux 的作者 Dan Abramov [总结](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)了组件类的几个缺点：
> 1. 复杂的组件难以拆分，也不便于测试。
> 2. 业务逻辑分散在组建的各个方法中，容易出现重复逻辑和关联逻辑，造成代码冗余。
> 3. 组件类引入了复杂的编程模式，比如 render、props 和高阶组件。

## 函数组件
**函数组件**顾名思义，就是用函数的形式来编写组件的内部逻辑和功能。但是，我们刚接触**面向对象编程**的时候就知道，**类是对一系列变量和方法的封装，其每个对象实例具有自己的状态属性**，而**函数只是一个过程，实现某种功能或者返回某个值**。

React 之前也有函数组件，如下：
```jsx
function HelloWorld(){
    return <div>Hello World !</div>
}
```
显然，这种函数组件虽然可以实现某种特定的功能，但是它**不包含状态，也不支持生命周期方法**，因此无法用于很多情景，更别说取代类了。

## Hooks
为了解决函数组件的不足之处，React 提出了 `Hooks` 的概念，即**通过 “钩子” 的形式把函数中需要的状态 ”钩“ 进来**。因此，我们利用 `React Hooks` 可以实现**完全不用“类”，写出一个全功能的组件**。

# Hooks 常用钩子
React Hooks 中有几种常用的钩子函数，分别是：
>**1. useEffect()**
>**2. useState()**
>**3. useRef()**
>**3. useContext()**
>**4. useReducer()**

下面，我们将分别介绍这五种常用钩子函数的作用。（至于其他钩子函数，可以自行百度或查看[官网文档](https://zh-hans.reactjs.org/docs/hooks-reference.html)）

## useEffect()：副作用钩子
`useEffect()` 顾名思义，“作用”钩子。这也是我们平时开发中最常用的钩子函数，往往需要实现某些“作用”但找不到对应钩子的时候，我们就可以用它。

那么 `useEffect()` 钩子的具体应用是什么呢？ 简单来说，当我们希望**组件加载或更新后实现一些其他功能**的时候，我们就可以用它。**组件每渲染一次，`useEffect()`函数就会自动触发一次**，至于是加载时触发还是更新后触发，要由我们设定的参数决定。

`useEffect()` 使用方式如下所示：
```jsx
import React, { useEffect } from 'react';

export default function HelloWorld(props) {
    // 关键代码
    useEffect(() => {
        document.title = 'Hello World !';
    }, []);
    return <h1>{document.title}</h1>;
}
```
其中，`useEffect(() => {}, [])` 中的**第一个参数是一个函数，即我们所需要实现的一些其他功能；第二个参数是一个 Array，如果为空，则只在组件挂载时触发一下，如果有相关属性值，则每当属性值改变就会触发一次**。如下所示：
```jsx
export default function HelloWorld(props) {
    useEffect(() => {
        document.title = props.title;
    }, [props.title]);
    return <h1>{document.title}</h1>;
}
```
上面的代码中，我们在第二个参数中设置了 `props.title`，即只要父组件传递过来的 `props.title` 发生了变化，这个子组件的 `document.title` 就会随之改变，重新渲染。**因此，如果我们不希望某个 `useEffect()` 每次渲染时都执行，我们可以在第二个参数 Array 中添加依赖项，只有依赖性发生变化时才会重新渲染**。

## useState()：状态钩子
`useState()` 也可以一眼看出大概是做什么用的，它就是给我们的函数组件**添加和修改状态**的钩子函数。因为函数组件没有状态，所以把需要的“状态”都存放在钩子里。

我们举一个例子，创建一个 `Button` 函数组件，并赋予其一个状态，即文本值，当触发点击操作时，文本状态值会改变。代码如下所示：
```jsx
import React, { useState } from "react";

export default function Button(props)  {
    // 关键代码
    const  [buttonText, setButtonText] =  useState('Hello World !');

    handleClick = (event) => {
        // 修改状态
        return setButtonText('你好，世界！');
    }

    return  <button  onClick={handleClick}>{buttonText}</button>;
}
```
从上面的示例代码可以看出，`useState()` 的用法为 `const [状态名, set状态名] = useState(初始值)`。其中状态名不难理解，就是我们赋予状态的变量名称。

后面的 “set状态名” 实际上就是**更新状态的函数的一个习惯性描述名称**，你也可以写出其他的名称，比如 `changeButtonText` 等等。但是为了提高代码的可读性， React 开发者一致约定将函数名写成 `set状态名` 的形式。当我们需要修改状态时，只需要 `set状态名(new state)` 即可更新。

## useRef()：DOM交互钩子
看到 `useRef()` 我们应该不陌生，因为 React 在类组件中提供 `createRef` 方法帮助我们**获取DOM节点**。而在 React Hooks 中则提供了 `useRef()` 钩子实现相同作用。

这里我们就简单介绍一下 `useRef()`。众所周知，一个网页中包含了若干DOM节点，当我们需要对某个DOM节点进行交互/操作时，我们可以通过如下方式**绑定DOM节点**。
```jsx
import React, {useRef} from 'React';

export default function HelloWorld(props) {
    // 关键代码
    const myRef = useRef({});
    return <button ref={myRef}>Click Me !</button>;
}
```
绑定完成之后，我们就可以**通过 `myRef.current` 来对该DOM节点进行相应的操作**。

## useContext()：共享状态钩子
新手入门 React 的时候知道组件间最简单的传递消息的方式是：**子组件通过`props` 的方式获取父组件的信息**。

然而，当我们需要传递信息的组件跨级过多，容易导致传递信息方式过于繁琐复杂，甚至容易出现很多错误，以及调试困难等问题。因此，React 也为我们提供了 `useContext()` 钩子函数实现**信息共享**。

（**原理与 props 大同小异**）

举个例子，我们希望组件 `School` 可以共享信息给 `Student` 和 `Teacher`。

第一步使用 React Context API，在组件外部建立一个 Context。
```jsx
export const SchoolContext = React.createContext({});
```
接着，我们利用 `School` 的 `Provider` 来封装组件 `Student` 和 `Teacher`。
```jsx
export default function School(props){
    return (
        // 核心代码
        <SchoolContext.Provider value={{studentName: 'douchen'}}>
            <div className='School'>
                <Student />
                <Teacher />
            </div>
        </SchoolContext.Provider>
    )
}
```
通过上面的代码，父组件 School 共享了 studentName 信息给 Student 和 Teacher 组件。那么 Student 和 Teacher 组件**如何取到这个值**呢？
```jsx
import React, { useContext } from "react";
import { SchoolContext } from './School.js';

export default function Student(props){
    // 核心代码
    const { studentName } = useContext(SchoolContext);

    return (
        <div className="student">
            <p>This student is {studentName}</p>
        </div>
    );

}
```
由上面代码可知，我们可以**先 `import`，然后通过 `useContext(SchoolContext)`** 来获取我们需要的值。

**！！！注意！！！**：到这儿可能就有人要问了，这为啥不直接用 `props` 呢？显然在这里用不用 props 都差不多。然而，一旦 `School` 的层级更高，而不只是 `Student` 和 `Teacher` 的父级，可能是更高的祖先级时，利用 props 一级一级传下去就会很复杂。

此外，我们还可以用另外一种眼界来看待 `useContext()`。如果**我们直接把 `Provider` 包在我们工程的最外层组件上，那就可以把最外层组件内的一些信息当作是全局信息，供所有子组件实用，那是不是很方便**？

## useReducer()：action 钩子
React 原本是不提供**状态管理器**的，更多的还是需要依赖外部库，如 `Redux`。但是，React Hooks 提供了 `useReducer()` 钩子函数实现状态管理。

什么是状态管理器呢？可以理解为这个组件它并**不负责UI，而是一个逻辑组件，用于管理其他组件间的逻辑关系和状态管理**。
```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```
上面的状态管理器接受 reducer 函数和状态的初始值作为参数，返回一个数组。数组的第一个成员是**状态的当前值**，第二个成员是**发送 action 的dispatch函数**。

举个例子，我们需要实现一个加减器，点击 + 按钮，计数加一；点击 - 按钮，计数减一。

首先，我们需要定义一个状态管理器 `countReducer`，分别讨论加减。
```jsx
const countReducer = (state, action) => {
    switch(action){
        case 'add':
            return{
                ...state,
                count: state.count + 1
            }
        case 'sub':
            return{
                ...state,
                count: state.count - 1
            }
        default:
            return state
    }
}
```
接着，我们的计数器UI组件代码如下：
```jsx
function Counter() {
    // 关键代码
    const [state, dispatch] = useReducer(countReducer, { count: 0 };

    return  (
        <div className="App">
            <p>Count: {state.count}</p>
            // 关键代码
            <button onClick={() => dispatch('add')}>+</button>
            <button onClick={() => dispatch('sub')}>-</button>
        </div>
    );
}
```
由上可知，`useReducer()` 可以帮助我们创建状态管理器，**只负责组件中的状态和各种逻辑的管理**。

不过，`useReducer()` 相比于 `Redux`，没法提供中间件（Middleware）和时间旅行（Time travel），如果你需要这两个功能，还是要用 [Redux](https://www.redux.org.cn/)。

(**个人还是推荐使用 Redux 来实现状态管理！**)

---
**很感谢你能看到这里！谢谢～**

---
参考资料： (衷心感谢各参考资料提供的帮助)
+  [React官方文档](https://zh-hans.reactjs.org/)
+  [React Hooks 入门教程](https://www.ruanyifeng.com/blog/2019/09/react-hooks.html)
+  [轻松学会 React 钩子：以 useEffect 为例](https://www.ruanyifeng.com/blog/2020/09/react-hooks-useeffect-tutorial.html)