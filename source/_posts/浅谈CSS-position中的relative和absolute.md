---
title: 浅谈CSS position中的relative和absolute
date: 2022-04-21 21:41:47
tags: 
    - 前端
    - CSS
categories: CSS  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/background/littlePrince3.png  # 文章页头部图片
cover: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/background/littlePrince3.png  # 主页中显示的文章封面图片
keywords: "CSS，position, relative, absolute"
---
# 前言
CSS 中的 `position` 属性指定了元素的定位类型，其有五个可选值，分别是`static`、`relative`、`absolute`、`fixed` 和 `sticky`。分别对应了**静态定位**、**相对定位**、**绝对定位**、**固定定位**和**粘性定位**。

这里我们主要探讨平时最常用的 `relative` 定位和 `absolute` 定位。网络上有很多相关的讲解，但还是通过自己的理解写一篇学习笔记会让自己的印象更深刻。因此，下面我将从自己的理解角度简单谈一谈，如果错误，请大家批评指正。（我也会把我参考的一些资料列在最后，大家可以自行查看）

# absolute 和 relative
下面，我将从**定义**、**定位基准**、**脱离文档流**和**内容宽度**几个角度进行介绍。
## 定义

**`absolute`** ：绝对定位，**脱离文档流**。即**相对于最近且不是 static 定位的父元素进行定位**。`position` 更多考虑的是**父级和子级间的关系**，即相对于父元素的定位，不受父元素内其他子元素的影响。

**`relative`** ：相对定位，**不脱离文档流**。即**保留自己原本位置，并相对于该位置进行定位**。`relative` 更多考虑的是**同级元素间的关系**，即考虑同级元素的定位后，得到自己的原本位置，并基于该位置进行相对的移动。


## 定位基准和文档流
### absolute
`absolute` 在定位时会**脱离文档流，不保留自己原本位置，而是相对于其最近且定位不是 static 的父元素进行定位**。如果没有这种父元素，则基于 `body` 定位。

举个例子，我们共有五个`<div>`，最外面一层的 `mydiv` 是其余四个 div 的父元素，即容器。代码如下：

```CSS
<style>
    .mydiv {
        position: relative;
        width: 100%;
    }
    .div1 {
        height: 50px;
        background-color: yellow;
    }
    .div2 {
        position: absolute;
        top: 20px;
        left: 20px;
        height: 50px;
        background-color: skyblue;
    }
    .div3 {
        height: 50px;
        background-color: lightgreen;
    }
    .div4 {
        height: 50px;
        background-color: pink;
    }
</style>
```
```html
<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='div2'>我是第二个div</div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>
```

效果如下：

<style>
    .mydiv {
        position: relative;
        width: 100%;
    }
    .div1 {
        height: 50px;
        background-color: yellow;
    }
    .div2 {
        position: absolute;
        top: 20px;
        left: 20px;
        height: 50px;
        background-color: skyblue;
    }
    .div3 {
        height: 50px;
        background-color: lightgreen;
    }
    .div4 {
        height: 50px;
        background-color: pink;
    }
</style>

<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='div2'>我是第二个div</div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>
</br>

我们将第二个 div 设置为 `absolute` 定位，并设置相应的偏移属性值 `top` 和 `left`。可以看到，**第二个 div 的偏移是基于其最近的父元素`mydiv` 的位置**，并且它的**原位置没有保留**，而是被第三个 div 补上了。

### relative

`relative` 在定位时**不脱离文档流，会保留自己原本位置，并相对于该位置进行定位**。

我们对上面的代码进行一定的修改，即把第二个 div 设置为 `relative` 定位。代码如下：

```CSS
<style>
    .div2 {
        position: relative;
        top: 20px;
        left: 20px;
        height: 50px;
        background-color: skyblue;
    }
</style>
```

效果如下：

<style>
    .div2-2 {
        position: relative;
        top: 20px;
        left: 20px;
        height: 50px;
        background-color: skyblue;
    }
</style>

<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='div2-2'>我是第二个div</div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>
<br>

我们将第二个 div 设置为 `relative` 定位，并设置相应的偏移属性值 `top` 和 `left`。可以看到，**第二个 div 的偏移是基于它原本位置的**，并且它的**原位置进行了保留**，从而覆盖到了一部分第三个 div。

此外，`absolute` 定位的元素如果没有指定长度时，其长度为内容长度。而 `relative` 定位的元素则会和父元素相同。

**-- 小Tips👇 --**

此外，`relative` 定位的元素还需要考虑到其父元素的内容。如给 div2 添加一个父元素 `container2`，并在其中添加文字内容。

```CSS
<style>
    .container2 {
        height: 50px;
        background-color: orange;
    }
</style>
```

```html
<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='container2'>我是container2
        <div class='div2'>我是第二个div</div>
    </div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>
```

<style>
    .container2 {
        height: 50px;
        background-color: orange;
    }
</style>

我们先看看 container2 中**没有文字**的效果是怎样的，如下：

<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='container2'>
        <div class='div2-2'>我是第二个div</div>
    </div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>
<br>

现在，我们给 container2 中**添加文字**，效果如下：
<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='container2'>我是container2
        <div class='div2-2'>我是第二个div</div>
    </div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>
<br>

由上面的对比我们可以发现，div2 作为container2 的子元素，**原本会出现在 container2 的文字下面**。因此，`top：20px` 的**偏移是基于它的原本位置的，而不是父元素**！故其相比于container2中没有文字的时候，稍微偏下一点，因为同级元素**文字**会占据一行高度。

而`absolute`定位则不会被同级元素影响，因为它是**完全基于最近的非static的父元素的位置进行定位的**。

## 内容宽度

1. `absolute` 定位的元素**宽度会根据内容变化**，不受父元素影响，也不会影响父元素。

2. `relative` 定位的元素**宽度会和父元素同步**。如果没有设置  relative 元素的宽度，则会跟其父元素宽度一致.

### absolute
`absolute` 定位的元素如果没有设置宽度，则会根据其内容进行自适应变化。效果如下：

<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='div2'>我是第二个div</div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>

### relative
`relative` 定位的元素如果没有设置宽度，则会和父元素一致。这里的父元素为 `mydiv`，宽度为 `100%`，效果如下：

<div class='mydiv'>
    <div class='div1'>我是第一个div</div>
    <div class='div2-2'>我是第二个div</div>
    <div class='div3'>我是第三个div</div>
    <div class='div4'>我是第四个div</div>
</div>
<br>

### absolute 父元素和 relative 子元素结合

但是要注意，**如果父元素是 `absolute` 定位的元素并没有设置宽度，此时 `relative` 定位的子元素的宽度会将父元素撑开**。而且，**父元素被撑开的宽度是根据其内容的宽度撑开的**，和relative移动的位置无关。

我们将上面的 style 样式修改一下，给每个 div 都设置一定的宽度，并验证撑开的效果。代码设置如下：
```CSS
<style>
    .myDiv {
        position: relative;
        width: 100%;
    }
    .Div1 {
        height: 50px;
        width: 200px;
        background-color: yellow;
    }
    .Container2 {
        position: absolute; /* 关键代码 */
        height: 50px;
        background-color: orange;
    }
    .Div2 {
        position: relative; /* 关键代码 */
        top: 20px;
        left: 20px;
        height: 50px;
        width: 400px;  /* 关键代码 */
        background-color: skyblue;
    }
    .Div3 {
        height: 50px;
        width: 200px;
        background-color: lightgreen;
    }
    .Div4 {
        height: 50px;
        width: 200px;
        background-color: pink;
    }
</style>
```
```html
<div class='myDiv'>
    <div class='Div1'>我是第一个div</div>
    <div class='Container2'>
        <div class='Div2'>我是第二个div</div>
    </div>
    <div class='Div3'>我是第三个div</div>
    <div class='Div4'>我是第四个div</div>
</div>
```
效果如下：

<style>
    .myDiv {
        position: relative;
        width: 100%;
    }
    .Div1 {
        height: 50px;
        width: 500px;
        background-color: yellow;
    }
    .Container2 {
        position: absolute;
        height: 50px;
        background-color: orange;
    }
    .Div2 {
        position: relative;
        top: 20px;
        left: 80px;
        height: 50px;
        width: 400px;
        background-color: skyblue;
    }
    .Div3 {
        height: 50px;
        width: 500px;
        background-color: lightgreen;
    }
    .Div4 {
        height: 50px;
        width: 500px;
        background-color: pink;
    }
</style>

<div class='myDiv'>
    <div class='Div1'>我是第一个div</div>
    <div class='Container2'>我是container2
        <div class='Div2'>我是第二个div</div>
    </div>
    <div class='Div3'>我是第三个div</div>
    <div class='Div4'>我是第四个div</div>
</div>
</br>

从上面效果可以得出结论：**因为 `absolute` 的父元素的宽度如果没有预先设定的话，则是根据内容来定义的。所以 `relative` 元素的宽度会撑开`absolute` 父元素。**

**注意！！！👇**
不过上面的例子有一点比较特殊，即按照 `absolute` 定位的原理来说，`Container2` 应当基于 `myDiv` 的左上角开始定位，即应该覆盖第一个 div。然而例子中 `Container2` 留在了原位，并且把上移的 `div3` 覆盖了。

原因是 **如果没有设置 `top`、`left`、`right` 和 `bottom` 时，`absolute` 则不会移动，与原文档流位置一致**。

## top 和 left 偏移
### absolute
`absolute` 定位可以根据`top`、`left`、`bottom` 和 `right` 来决定元素相对于定位基准的偏移。

`top` 表示元素上边框与定位基准的顶部的距离，`bottom` 表示元素下边框与基准底部的距离。如果两者同时存在，**只有 top 起作用**；如果两者都未指定，则**其顶部与原文档流位置一致**（就是上面注意点提到的），即垂直位置保持不变。

`left` 表示元素左边框与定位基准左边的距离，`right` 表示元素右边框与基准右边的距离。两者同时存在时，**只有 left 起作用**；如果两者都未指定，则**其左边将与原文档流位置一致**，即水平保持位置不变。 

### relative
`absolute` 定位只可以根据`top`、`left` 来决定元素相对于父元素的偏移。`bottom` 和 `right` **不起效**。

---
**很感谢你能看到这里！谢谢～**

---
参考资料： (衷心感谢参考资料中各博主的帮助)
+  [Absolute（绝对定位）与relative（相对定位）的图文讲解](https://blog.csdn.net/weixin_42067967/article/details/80152403)
+  [CSS+DIV布局中absolute和relative区别](https://www.cnblogs.com/llljs666/p/10579440.html)