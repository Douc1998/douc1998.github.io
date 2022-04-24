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
>**3. useContext()**
>**4. useReducer()**

下面，我们将分别介绍这四种常用钩子函数的作用。

## useEffect()：副作用钩子
`useEffect()` 顾名思义，“作用”钩子。这也是我们平时开发中最常用的钩子函数，往往需要实现某些“作用”但找不到对应钩子的时候，我们就可以用它。

那么 `useEffect()` 钩子的具体应用是什么呢？ 简单来说，当我们希望**组件加载或更新后实现一些其他功能**的时候，我们就可以用它。**组件每渲染一次，`useEffect()`函数就会自动触发一次**，至于是加载时触发还是更新后触发，要由我们设定的参数决定。

`useEffect()` 使用方式如下所示：
```jsx
import React, { useEffect } from 'react';

export default function HelloWorld(props) {
  useEffect(() => {
    document.title = 'Hello World !';
  }, []);
  return <h1>{document.title}</h1>;
}
```
其中，`useEffect(() => {}, [])` 中的**第一个参数是一个函数，即我们所需要实现的一些其他功能；第二个参数是一个 Array，如果为空，则只在组件挂载时触发一下，如果有相关属性值，则每当属性值改变就会触发一次**。

## useState()：状态钩子

## useContext()：共享状态钩子

## useReducer()：action 钩子