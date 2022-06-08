---
title: CSS中Hover选择器如何控制其他元素？
date: 2022-04-21 16:10:50
tags: 
     - 前端
     - CSS
categories: CSS
comments: true
toc: true
top_img: https://fastly.jsdelivr.net/gh/Douc1998/CDN-source/image/background/space13.jpeg
cover: https://fastly.jsdelivr.net/gh/Douc1998/CDN-source/image/background/space13.jpeg
keywords: "CSS, Hover"
---
# 前言
写项目的时候遇到一个需求，大概意思就是：**当鼠标停留在元素A身上时，如何让元素A的子元素B显现（或其他变化）**？其实这个需求很简单啦！无非就是以下两个流程：（以transition为例）
>1. 给子元素B添加`transition`过渡参数。
>2. 在元素A的伪类 `:hover` 选择器中，给子元素B添加相应的过渡属性值。

但是为什么还要写一篇博客来记录这个东西呢？因为刚好接着这个问题**总结一下 `hover` 的控制其他元素的几种情景及其对应方法**。

# Hover选择器如何控制其他元素？
Hover 如何控制其他元素的意思就是 **当在对元素A进行hover动作时，如果控制或让其他元素进行变化**，这属于是`CSS`的范畴。这个问题主要包括三个情景，分别是：
> 1. Hover 元素A，控制其子元素B
> 2. Hover 元素A，控制其同级元素C
> 3. Hover 元素A, 控制其同级元素C的子元素D

下面我们将一一进行讨论：
## 控制子元素
我们先举一个例子：**父元素A为 100x100的蓝色方块，子元素B为50x50的绿色方块，当我们鼠标移入蓝色方块中时，绿色方块显示，移出则不显示**。

为了实现以上功能，我们需要使用到**CSS的父元素 `:hover` 控制子元素**的方法，如下所示：

```CSS
    .父元素名:hover .子元素名 {
        /* 其他内容 */
    }
```
我们只需要在关键代码中添加我们需要的变化内容即可！效果如下：

<style>
    /* 父元素A，大小100x100，背景蓝色 */
    .fatherA {
        height: 100px;
        width: 100px;
        background-color: blue;
    }
    /* 子元素B，大小50x50，背景绿色，默认不显示 */
    .childB {
        visibility: hidden;
        height: 50px;
        width: 50px;
        background-color: green;
    }
    /* 关键代码 */
    .fatherA:hover .childB {
        visibility: visible
    }
</style>
<div class='fatherA'>
    <div class= 'childB'></div>
</div>
完整代码如下：
```CSS
<style>
    /* 父元素A，大小100x100，背景蓝色 */
    .fatherA {
        height: 100px;
        width: 100px;
        background-color: blue;
        margin: auto;
    }
    /* 子元素B，大小50x50，背景绿色，默认不显示 */
    .childB {
        visibility: hidden;
        height: 50px;
        width: 50px;
        background-color: green;
    }
    /* 关键代码 */
    .fatherA:hover .childB {
        visibility: visible
    }
</style>
```
```HTML
<div class='fatherA'>
    <div class= 'childB'></div>
</div>
```

## 控制同级元素
刚刚说到了控制子元素，那我们如何控制同级元素呢？

还是举一个例子：**元素A为 100x100的蓝色方块，同级元素C为100x100的红色方块，当我们鼠标移入蓝色方块中时，红色方块显示，移出则不显示**。

为了实现以上功能，我们需要使用到**CSS元素 `:hover` 控制同级元素**的方法。相比于控制子元素，控制同级元素的方法在**被控制的元素名前多了一个 “+” 号**。如下所示：

```CSS
    .元素名:hover +.同级元素名 {
        /* 其他内容 */
    }
```
我们只需要在关键代码中添加我们需要的变化内容即可！效果如下：

<style>
    /* 元素A，大小100x100，背景蓝色 */
    .brotherA {
        display: inline-block;
        height: 100px;
        width: 100px;
        background-color: blue;
    }
    /* 同级元素C，大小100x100，背景红色，默认不显示 */
    .brotherC {
        display: none;
        height: 100px;
        width: 100px;
        background-color: red;
    }
    /* 关键代码 */
    .brotherA:hover +.brotherC {
        display: inline-block;
    }
</style>
<div class='brotherA'></div>
<div class= 'brotherC'></div>

完整代码如下：
```CSS
<style>
    /* 元素A，大小100x100，背景蓝色 */
    .brotherA {
        display: inline-block;
        margin: auto;
        height: 100px;
        width: 100px;
        background-color: blue;
    }
    /* 同级元素C，大小100x100，背景红色，默认不显示 */
    .brotherC {
        display: none;
        height: 100px;
        width: 100px;
        background-color: red;
    }
    /* 关键代码 */
    .brotherA:hover +.brotherC {
        display: inline-block;
    }
</style>
```
```HTML
<div class='brotherA'></div>
<div class= 'brotherC'></div>
```

## 控制同级元素的子元素
还有一种相比于上面两种情景更复杂一点的情景，即**控制同级元素的子元素**。

举一个例子：**元素A为 100x100的蓝色方块，同级元素C为100x100的红色方块，元素C有一个子元素D，为50x50的黄色方块。当我们鼠标移入蓝色方块中时，黄色方块显示，移出则不显示**。

为了实现以上功能，我们需要使用到**CSS元素 `:hover` 控制同级元素的子元素**方法。相比于控制同级元素，控制同级元素的子元素时还需要**在同级元素名后添加其子元素名**。如下所示：

```CSS
    .元素名:hover +.同级元素名 .子元素名 {
        /* 其他内容 */
    }
```
我们只需要在关键代码中添加我们需要的变化内容即可！效果如下：

<style>
    /* 元素A，大小100x100，背景蓝色 */
    .fatherA2 {
        display: inline-block;
        height: 100px;
        width: 100px;
        background-color: blue;
    }
    /* 同级元素C，大小100x100，背景红色 */
    .fatherC2 {
        display: inline-block;
        height: 100px;
        width: 100px;
        background-color: red;
    }
    /* 同级元素C的子元素D，大小50x50，背景红色 */
    .childD {
        visibility: hidden;
        height: 50px;
        width: 50px;
        background-color: yellow;
    }
    /* 关键代码 */
    .fatherA2:hover +.fatherC2 .childD{
        visibility: visible
    }
</style>
<div class='fatherA2'></div>
<div class= 'fatherC2'>
   <div class='childD'></div>
</div>

完整代码如下：
```CSS
<style>
    /* 元素A，大小100x100，背景蓝色 */
    .fatherA {
        display: inline-block;
        height: 100px;
        width: 100px;
        background-color: blue;
    }
    /* 同级元素C，大小100x100，背景红色 */
    .fatherC {
        display: inline-block;
        height: 100px;
        width: 100px;
        background-color: red;
    }
    /* 同级元素C的子元素D，大小50x50，背景红色 */
    .childD {
        visibility: hidden;
        height: 50px;
        width: 50px;
        background-color: yellow;
    }
    /* 关键代码 */
    .fatherA:hover +.fatherC .childD{
        visibility: visible
    }
</style>
```

```HTML
<div class='fatherA'></div>
<div class= 'fatherC'>
   <div class='childD'></div>
</div>
```
以上就是 **CSS中Hover选择器控制其他元素的三种情景及其对应的实现方法**了。内容还是比较简单的。以此作为记录，以备以后查看，也希望对你有所帮助！

---
**很感谢你能看到这里！谢谢～**
