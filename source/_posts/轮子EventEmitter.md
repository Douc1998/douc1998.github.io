---
title: 造轮子系列（1）：EventEmitter
date: 2022-05-05 10:02:05
tags: 
    - 前端
    - 轮子
    - JavaScript
categories: 轮子  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://gcore.jsdelivr.net/gh/Douc1998/CDN-source/image/background/littlePrince4.png  # 文章页头部图片
cover: https://gcore.jsdelivr.net/gh/Douc1998/CDN-source/image/background/littlePrince4.png  # 主页中显示的文章封面图片
keywords: "EventEmitter, Wheel, JavaScript"
---
# 前言

`EventEmitter` 可以提供我们实现**事件的发布和订阅**。

`Node.js` 已经提供了封装好的 `EventEmitter` 供我们使用，通过 `events` 模块可以使用。
```js
const eventEmitter = require('events').EventEmitter;
```
然而，自己动手实现 `Wheel` （造轮子）虽然不能够做到已有库那般稳定、高性能，但是**有利于我们去了解其内部机理**。

**下面我们自己实现了一个简易的[EventEmitter](https://github.com/Douc1998/EventEmitter-Wheel/blob/main/EventEmitter/EventEmitter.js)。**

# 准备工作

这里我是通过 `JavaScript` 语言实现的，利用到了少量 `ES6` 标准中的新内置变量。因此，希望你对这俩有一些基础的了解和基本应用能力。

(这里我推荐阮一峰老师的[教程](https://www.ruanyifeng.com/blog/2017/09/es6_primer_3rd_edition.html))
>+ **JavaScript**
>+ **ECMAScript 6**

# 实现原理
`EventEmitter` 的实现原理还是比较简单的，用一句话概括就是：**基于事件名称存储对应的方法，并通过事件名称调用方法。**

其实，但凡关于**事件的订阅与发布**，其最基本原理皆是如此，不过是实现方法的差异而已。

因为我们需要重新造轮子，所以我们根据上述原理，可以把实现流程概括如下：

>1. 创建存储所有事件及方法的字典 `events` {}
>2. 发布事件时，提供事件名称 `eventName` 和方法 `func`，并添加到 `events` 中。
>3. 触发事件时，提供事件名称 `eventName` 和方法参数 `args`，在 `events` 中找到对应方法并执行。
>4. 卸载事件时，提供事件名称 `eventName` 和方法 `func`，在 `events` 中找到对应内容进行删除。

实现代码如下：
```js
// EventEmitter Wheel 事件订阅与发布
export const EventEmitter = new class EventEmitter{
    constructor(){
        // 记录订阅事件
        this.events = {}
    }

    static VERSION = '1.0.0'

    /**
     * 判读是不是function方法
     * @param {Function / Object} func 
     * @returns 
     */
    isFunction = (func) => {
        if (typeof func === 'function') {
            return true
        } else if (typeof func === 'object') {
            return this.isFunction(func.func)
        } else {
            return false
        }
    }

    /**
     * 添加事件
     * @param {String} eventName 事件名称
     * @param {Function} func 函数方法
     * @returns {Object} this
     */
    on = (eventName, func) => {
        try{
            if(!eventName || !func) throw new Error('Please ensure that the parameters are complete !');
            if(!this.isFunction(func)) throw new Error('The second parameter must be a function or an Object with func!');
            // 如果满足要求则加入，
            (this.events[eventName] || (this.events[eventName] = [])).push(
                typeof func === 'object' ? 
                func :
                {
                    func: func,
                    once: false
                });
        } catch(err){
            console.log(err)
        }
        return this;
    }

    /**
     * 添加只能触发一次的事件
     * @param {String} eventName 事件名称
     * @param {Function} func 函数方法
     * @returns {Object} this
     */
    once = (eventName, func) => {
        return this.on(eventName, {
            func: func,
            once: true
        })
    }

    /**
     * 删除事件
     * @param {String} eventName 事件名称
     * @param {Function} func 函数方法
     * @returns {Object} this
     */
    off = (eventName, func) => {
        if(!eventName || !this.events[eventName]) return this; // 如果没输入eventName或不存在，则return
        if(!func){  // 如果没有输入某个func，则删除该eventName对应的所有内容
            this.clear(eventName)
        }else{ // 删除指定数组下的指定方法
            const funcs = this.events[eventName];
            for(let i = 0; i < funcs.length; i++){
                if(funcs[i].func === func){
                    funcs.splice(i, 1)
                }
            }
            if(funcs.length === 0){ // 如果该事件对应的数组为空，则删除该事件，释放空间
                this.clear(eventName)
            }
        }
        return this;

    }

    /**
     * 删除某一个事件下所有内容或者清除所有事件
     * @param {String} eventName 
     */
    clear = (eventName) => {
        if(eventName && this.events[eventName]){  // 直接清除某个事件下所有内容
            Reflect.defineProperty(this.events, eventName)
        }else{ // 不输入 eventName 则清空所有事件
            this.events = {}
        }
    }

    /**
     * 触发事件
     * @param {String} eventName 事件名称
     * @param  {...any} args 形参
     * @returns {Object} this
     */
    emit = (eventName, ...args) => {
        try{
            const funcs = this.events[eventName];
            if(!funcs) throw new Error(eventName, 'is not existed');;
            funcs.forEach(element => {
                element.func.call(this, ...args);
                // 如果是 once，执行完之后要 off
                if (element.once) {
                    this.off(eventName, element.func)
                }
            });
        }catch(err){
            console.log(err);
        }
        return this;
    }
}()
```

# 基础功能

## on
添加一个事件并发布。
```js
eventEmitter.on(eventName, func)
```
>+ eventName 事件名称
>+ func 监听函数

## emit
触发某个事件。
```js
eventEmitter.emit(eventName, ...args)
```
>+ eventName 事件名称
>+ args 监听函数的形参

## off
删除已发布的一个事件。
```js
eventEmitter.off(eventName, func)
```
>+ eventName 事件名称
>+ func 监听函数

## once
添加一个只能触发一次的事件，执行后将自动被删除。
```js
eventEmitter.once(eventName, func)
```
>+ eventName 事件名称
>+ func 监听函数

## clear
清除一个事件或所有事件。
```js
eventEmitter.clear(eventName)
```
>+ eventName 事件名称

# 使用案例

## 导入

```js
import { eventEmitter } from 'path/EventEmitter.js';
```
或
```html
<script src='path/eventEmitter.js'></script>
```

## 使用

```js
// 1. --- 添加事件 ---
eventEmitter.on('sayHello', (name) => {
    console.log('Hello! ' + name.toString() + '!')
});



// 2. --- 触发事件 ---
const Name = 'Douchen';
eventEmitter.emit('sayHello', Name);  // Hello! Douchen! 



// 3. --- 添加只能触发一次的事件 ---
eventEmitter.once('sayBye', (name) => {
    console.log('ByeBye! ' + name.toString() + '!')
});

// sayBye 事件只能触发一次！
eventEmitter.emit('sayBye', Name);  // ByeBye! Douchen!
eventEmitter.emit('sayBye', 'Superman');  // Error: sayBye is not existed



// 4. --- 卸载事件，如果没有func参数，则删除事件下所有funcs ---
eventEmitter.off('sayHello', (name) => {
    console.log('ByeBye! ' + name.toString() + '!')
});
console.log(eventEmitter.events);  // {}



// 5. --- 清除事件 ---
eventEmitter.on('planA', () => {
    console.log('This is planA');
});
eventEmitter.on('planB', () => {
    console.log('This is planB');
});
eventEmitter.on('planC', () => {
    console.log('This is planC');
});
// 清除 planA
eventEmitter.clear('planA');
console.log(eventEmitter.events); // {planB: Array(1), planC: Array(1)}
// 如果不指定 eventName，则清空 events
eventEmitter.clear();
console.log(eventEmitter.events); // {}
```

---
**很感谢你能看到这里！谢谢～**

博主已经将源码文件上传到了 Github 中，如有需要请自取 👉 [EventEmitter](https://github.com/Douc1998/EventEmitter-Wheel/blob/main/EventEmitter/EventEmitter.js)

---
参考资料： (衷心感谢各参考资料提供的帮助)
+  [JavaScript 中文手册](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
+  [冴羽 Github](https://github.com/mqyqingfeng/EventEmitter)
