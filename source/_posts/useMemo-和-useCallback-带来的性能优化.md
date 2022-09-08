---
title: useMemo 和 useCallback 带来的性能优化
date: 2022-09-07 09:44:33
tags: 
    - 前端
    - React
categories: React  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://gcore.jsdelivr.net/gh/Douc1998/CDN-source/image/background/scene5.png  # 文章页头部图片
cover: https://gcore.jsdelivr.net/gh/Douc1998/CDN-source/image/background/scene5.png  # 主页中显示的文章封面图片
keywords: "React, Hooks, 前端, JavaScript"
---

# 前言
随着**函数式组件**的产生，`React Hooks` 给我们带来了很多便利和新功能。先前我在[《React Hooks 入门》](https://blog.douchen.life/React-Hooks%E5%85%A5%E9%97%A8/)这篇文章中简单介绍了几种常用的 `Hooks`。今天，本篇文章将谈一谈 `useCallback` 和 `useMemo` 两个钩子函数。

# useCallback

首先我们先看一下[官方文档](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)中对 `useCallback` 的说明：
> Pass an inline callback and an array of dependencies. useCallback will return a memoized version of the callback that only changes if one of the dependencies has changed.

简单来说，它和 `useEffect` 的基本原理类似：有两个参数，第一个参数是 **内联回调函数**，第二个是 **依赖项数组**。它将返回该回调函数的 `memoized` 版本，该回调函数仅在某个依赖项改变时才会更新。

一句话总结：**缓存函数，仅在依赖项改变的时候才会更新。** 由此可知， `useCallback` 可以有利于我们判断历史和当前的内联回调函数的相等性，从而避免非必要渲染（例如 `shouldComponentUpdate`）的子组件。因为我们知道，即使两个内容完全相同的函数，在 js 中判等也是 false。

下面我们举个栗子：

`App.js` 入口文件，作为**父组件**。

```js
// App.js 
import React, { useState, useCallback } from 'react';
import RandomNum from './RandomNum';

function App(props) {
  const [count1, setCount1] = useState(0);
  const [count2, setCount2] = useState(0);

  const handleCount1 = () => {
    setCount1(count1 + 1);
  }

  const handleCount2 = useCallback(() => {
    setCount2(count2 + 1);
  }, [count2])


  return (
    <>
      <div>
        <RandomNum onClickButton={handleCount1}>Button1</RandomNum>
      </div>
      <div>
        <RandomNum onClickButton={handleCount2}>Button2</RandomNum>
      </div>
    </>
  )

}

export default App;
```

`RandomNum.js` 为子组件，包括：**点击按钮** 和 **随机数**。

```js
// RandomNum.js
import React from 'react';

const RandomNum = ({ onClickButton, children }) => {
  return (
    <>
      <button onClick={onClickButton}>{children}</button>
      <span>{Math.random()}</span>
    </>
  );
};

export default RandomNum;
```

上述代码的实验结果如下：

> 1. 点击 Button1，只有 Button1 后面的随机数改变。
> 2. 点击 Button2，Button1 和 Button2 后面的随机数均改变。

详细实验结果如下所示：
1、初始化结果：

![初始值](./useMemo-和-useCallback-带来的性能优化/origin.png)

2、点击 Button1 后，Button1 后面的随机数改变。

![点击 Button1](./useMemo-和-useCallback-带来的性能优化/button1.png)

3、点击 Button2 后，Button1 和 Button2 后面的随机数都改变。

![点击 Button2](./useMemo-和-useCallback-带来的性能优化/button2.png)

了解过 `React` 的渲染机制的应该都知道：**当父组件更新时，子组件也会跟着重新渲染**。因此，当点击 `Button1` 或者 `Button2`，会导致 `App` 这个父组件更新，从而导致两个子组件也更新。但是 `Button2` 的点击事件通过 `useCallback` 封装了起来，**仅仅在 `count2` 改变时，才会更新内联函数**。而 `Button1` 的点击事件会在父组件更新后也跟着更新，重新触发更新后的事件。

由此可知，点击 `Button1` 会导致其后面的随机数改变，而 `Button2` 的随机数不会改变，因为 `Button2` 没有更新；点击 `Button2` 会导致两个按钮后的随机数都改变，因为两个按钮都更新了。


# useMemo

首先我们先看一下[官方文档](https://zh-hans.reactjs.org/docs/hooks-reference.html)中对 `useMemo` 的说明：

> Pass a “create” function and an array of dependencies. useMemo will only recompute the memoized value when one of the dependencies has changed.

简单来说，`useMemo` 的参数包括一个创建函数和依赖项。**创建函数会需要返回一个值，只有在依赖项发生改变的时候，才会重新调用此函数，返回一个新的值**。

举个栗子，如下所示：

```js
// 使用 useMemo

import React, { useState, useMemo } from 'react';
import User from './User';

function App(props){
    const [age, setAge] = useState(0);
    const userInfo = useMemo(() => {
        return {
            name: 'jack',
            age: age,
            // some other information
        }
    }, [age]);
    return (
        <User info={userInfo}>
    )
}
```
```js
// 不使用 useMemo

function App(props){
    const [age, setAge] = useState(0);
    const userInfo = {
            name: 'jack',
            age: age,
            // some other information
    }
    return (
        <User info={userInfo}>
    )
}
```
从上面的栗子可以看出，第一段代码使用了 `useMemo`，即只有当 `age` 属性改变时，`userInfo` 的值才会改变，子组件 `User` 才会重新渲染；第二段代码没有使用 `useMemo`，是要父组件 `App` 重新渲染，子组件 `User` 也会跟着渲染。

因此，**使用 `useMemo` 可以减少一些不必要的渲染，从而提升性能**。继续想下去，那么我们可以把一些具有昂贵的逻辑运算或数据处理放到 `useMemo` 中，实现**缓存**，这样可以大大减少每次渲染的开销。

如果没有提供依赖项数组，`useMemo` 在每次渲染时都会计算新的值，这和 `useEffect` 没有提供依赖项数组是同样的处理情况。

肯定会有人问这个 `useMemo` 和 `useEffect` 有什么关系，看起来挺像的。记住，**传入 `useMemo` 的函数会在渲染期间执行。请不要在这个函数内部执行不应该在渲染期间内执行的操作，诸如副作用这类的操作属于 `useEffect` 的适用范畴，而不是 `useMemo`**。

# 总结
`useCallback` 可以**封装函数，以保存内联函数的不变性**，即缓存函数。可以利用引用判等性和 `shouldComponentUpdate` 结合使用子组件避免一些不必要的渲染。

`useMemo` 缓存的是**函数返回值**，它既可以优化子组件的不必要渲染，也可以存储当前组件的一些复杂逻辑计算环节。


---
**很感谢你能看到这里！谢谢～**

---
参考资料： (衷心感谢各参考资料提供的帮助)
+  [React 官方文档](https://zh-hans.reactjs.org/docs/hooks-reference.html)
+  [详解 React useCallback & useMemo](https://juejin.cn/post/6844904101445124110)





