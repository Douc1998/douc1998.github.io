---
title: CSS几种水平垂直居中方法
date: 2022-07-24 09:54:24
tags:
    - 前端
    - CSS
categories: CSS  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://gcore.jsdelivr.net/gh/Douc1998/CDN-source/image/background/space13.jpeg  # 文章页头部图片
cover: https://gcore.jsdelivr.net/gh/Douc1998/CDN-source/image/background/space13.jpeg  # 主页中显示的文章封面图片
keywords: "CSS，过渡动画"
---

# 前言
今天我们讲一个简单而又常用的问题：**居中问题**。

对于我们绝大多数人来说，对称是一种美，包括我们身边的各种事物，甚至说我们自己本身，都是对称的。既然说到对称，那我们也很容易引申出**居中**这一概念。

在前端设计中，居中也是一个很普遍的需求。当然，实现居中的方法也是层出不穷，只有你想不到，没有前端工程师做不到的哈哈哈。因此，本文章将总结一些**常用的元素居中方法**。

# 常用的元素居中方法
根据自己的理解和日常的使用，我主要总结了 8 种居中方法，读者可以根据自己的需求自行选取。
> 1. absolute + 负 margin
> 2. absolute + margin: auto
> 3. absolute + calc
> 4. absolute + transform
> 5. text-align + vertical-align + line-height
> 6. 基于 table 布局
> 7. 基于 grid 布局
> 8. 基于 flex 布局

首先我们先定义两个元素，即：父元素（容器） `container` 和 子元素 `element`。其 html 组织形式如下所示，后续也将不再重复赘述这一部分。
```html
<div class='container'>
    <div class='element'></div> 
</div>
```
## absolute + 负 margin
我们都知道 `position: absolute` 是绝对位置，其提供了 `left`、`right`、`top`、`bottom` 等参数来进行元素的定位。如果有不了解的建议去查看我的另外一篇[文章](https://blog.douchen.life/浅谈CSS-position中的relative和absolute/)。

因为 absolute 需要一个基准父元素，因此我们可以给父元素的 position 属性设置为 relative，这样让子元素的定位基准为该父元素。

此外，我们还知道 left、top 偏移**是基于子元素的左上角和父元素的左上角的位置关系**，如果把这两个值都设置为 50%，那么实际情况是子元素的左上角在父元素的正中间。显然，这并不是居中，所以可以再让子元素朝着 left 和 top 方向移动**负的自身一半长度和宽度的距离**。代码如下：

```css
.container {
    position: relative;
    width: 200px;
    height: 200px;
    border: 2px solid black;
}
.element {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 50%; /* 关键代码 */
    left: 50%; /* 关键代码 */
    margin-top: -50px; /* 关键代码 */
    margin-left: -50px; /* 关键代码 */
    background-color: orange;
}
```
<style>
    .container1 {
        position: relative;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element1 {
        position: absolute;
        width: 100px;
        height: 100px;
        background-color: orange;
        top: 50%;
        left: 50%;
        margin-top: -50px;
        margin-left: -50px;
    }
</style>
<div class='container1'>
    <div class='element1'></div> 
</div>
<br>

从上面的效果图可以看出，橙色的方块确实实现了水平和垂直方向的居中。然而，这种方法的缺点也比较明显，即**必须事先知道元素的宽高**，才能实现负 margin 值的设定。

## absolute + margin: auto
第二种方法也是基于 absolute 和 margin 的，但是我们知道 `margin: auto` 可以实现剩余空间的**平均分配**。

当没有设置宽高时，把 `left`、`right`、`top`、`bottom` 属性设置为 0 时，子元素会被拉伸到和父元素一样的大小。然而，当我们后续把一定宽高的子元素扔进去，并利用 `margin: auto`，就可以实现上下左右剩余空间的平均分配，从而实现子元素的居中显示。（如有不理解的建议去了解一下盒子模型的 margin: auto原理 和 absolute 定位的原理）。代码如下：

```css
.container {
    position: relative;
    width: 200px;
    height: 200px;
    border: 2px solid black;
}
.element {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 0; /* 关键代码 */
    left: 0; /* 关键代码 */
    bottom: 0; /* 关键代码 */
    right: 0; /* 关键代码 */
    margin: auto; /* 关键代码 */
    background-color: red;
}
```

<style>
    .container2 {
        position: relative;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element2 {
        position: absolute;
        width: 100px;
        height: 100px;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        margin: auto;
        background-color: red;
    }
</style>
<div class='container2'>
    <div class='element2'></div> 
</div>
<br>

上图的效果满足我们的需求，并且这种方法对子元素的移动并**不基于子元素的宽高**，是一种**较为优雅且普适**的方法。

## absolute + calc
这种方法十分类似于第一种方法。然而，使用 `calc()` 就不需要像第一种方法一一定义 `margin-top` 和 `margin-right` 了，只需要直接将 top 和 left 中设置为移动到正中心的距离即可。代码如下：

```css
.container {
    position: relative;
    width: 200px;
    height: 200px;
    border: 2px solid black;
}
.element {
    position: absolute;
    width: 100px;
    height: 100px;
    top: calc(50% - 50px); /* 关键代码 */
    left: calc(50% - 50px); /* 关键代码 */
    background-color: green;
}
```
<style>
    .container3 {
        position: relative;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element3 {
        position: absolute;
        width: 100px;
        height: 100px;
        background-color: green;
        top: calc(50% - 50px);
        left: calc(50% - 50px);
    }
</style>
<div class='container3'>
    <div class='element3'></div> 
</div>
<br>

但是，这种方法虽然比第一种方法看起来更为简便，但是缺点也相同：**必须事先知道元素的宽高**，才能实现 top、left 值的设定。

## absolute + transform

第四种方法还是基于 absolute 定位，但是 CSS3 新特性 `transform` 中的 `translate` 属性也可以设置百分比，其值是基于自身的宽高。因此，可以将 translate 设置为 -50%，实现居中。代码如下：

```css
.container {
    position: relative;
    width: 200px;
    height: 200px;
    border: 2px solid black;
}
.element {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 50%; /* 关键代码 */
    left: 50%; /* 关键代码 */
    transform: translate(-50%, -50%); /* 关键代码 */
    background-color: blue;
}
```
<style>
    .container4 {
        position: relative;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element4 {
        position: absolute;
        width: 100px;
        height: 100px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: blue;
    }
</style>

<div class='container4'>
    <div class='element4'></div> 
</div>
<br>

这种方法的好处更加显而易见：**定位移动都是基于百分比的，不依赖元素的宽高**。这种方法也是比较优雅且普适的。

## text-align + vertical-align + line-height

我们知道 `text-align: center` 能够让元素中的文字水平居中。当然这只是它的其中之一作用，当我们在**块级元素**下使用 `text-align: center`，可以使得**包含在其中的行内元素或行内块元素水平居中对齐**，也就可以实现所需要的水平居中了。

`vertical-align: middle` 对于其子元素而言，就是让**子元素的垂直中心点与行框盒子基线往上 1/2 x-height 处对齐**。这里涉及到一个 x-height 比较复杂，如果你对此感兴趣，可以参考张鑫旭大佬的文章：[CSS深入理解vertical-align和line-height的基友关系](https://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/)。

如果你并不想了解太多，那么简单来说，`vertical-align: middle` 就是为了让子元素垂直居中，但是它在垂直方向上居中位置是和一行文字的正中间心对齐，然而一行文字的高度就是`line-height`。所以如果巧妙地把父元素的 `line-height` 设置为父元素的高度值，再使用 `vertical-align: middle`，就可以实现子元素的垂直居中！

> `vertical-align` 还有其他的可选值，如 top、bottom、baseline 等，意思就是分别让子元素的 top、bottom、baseline 位置和行内文字的 top、bottom、baseline位置对齐。这里涉及到行内文字的相关的知识，建议学习一下，不然没办法理解。（教程：[line-height详解](https://blog.csdn.net/liting_1992/article/details/120888328))

废话不多说，暂不谈 top、bottom、baseline 等，回到我们的居中！代码如下：

```css
.container {   
    text-align: center; /* 关键代码 */
    line-height: 200px; /* 关键代码 */
    width: 200px;
    height: 200px;
    border: 2px solid black;
}
.element {
    display: inline-block; /* 关键代码 */
    vertical-align: middle; /* 关键代码 */
    width: 100px;
    height: 100px;
    background-color: purple;
}
```
<style>
    .container5 {
        text-align: center;
        line-height: 200px;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element5 {
        display: inline-block;
        vertical-align: middle;
        width: 100px;
        height: 100px;
        background-color: purple;
    }
</style>
<div class='container5'>X
    <div class='element5'></div> 
</div>
<br>

我在上面的图片中特地加了一个 “ X ” 字母，其实也可以看出来，`line-height` 设置为父元素的高度后，行内文字 X 的垂直方向正中心就在父元素的垂直居中位置，所以我们的子元素（紫色方块）与之对齐，从而达到了垂直的居中。

这种方法的好处就是也**不依赖于子元素的宽高**，但是从上面分析的过程也能看出来这个方法有些**麻烦且不易于理解**。


## 基于 table 布局

CSS 提供了 **table** 属性，因此我们可以把父容器类比为 table 中的每一个 **table—cell**，并让其内部元素的垂直和水平方向都设置为居中，从而实现居中效果。代码如下：

```css
.container {   
    display: table-cell; /* 关键代码 */
    text-align: center; /* 关键代码 */
    vertical-align: middle; /* 关键代码 */
    width: 200px;
    height: 200px;
    border: 2px solid black;

}
.element {
    display: inline-block; /* 关键代码 */
    width: 100px;
    height: 100px;
    background-color: pink;
}
```
<style>
    .container6 {
        display: table-cell; 
        text-align: center;
        vertical-align: middle;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element6 {
        display: inline-block;
        width: 100px;
        height: 100px;
        background-color: pink;
    }
</style>
<div class='container6'>
    <div class='element6'></div> 
</div>
<br>

这种方法不依赖于子元素，而且**灵活性和兼容性都比较高**，是一种优雅且普适的方法！

## 基于 grid 布局

既然第六点提到了利用 table 属性，那么自然而然地也可以想到最新的 **grid** 布局。使用这种方法也很简洁方便，逻辑简单易理解，只需要设置 `justify-self: center` 和 `align-self: center` 属性即可。代码如下：

```css
.container {
    display: grid; /* 关键代码 */
    width: 200px;
    height: 200px;
    border: 2px solid black;
}
.element {
    justify-self: center; /* 关键代码 */
    align-self: center; /* 关键代码 */
    width: 100px;
    height: 100px;
    background-color: lightblue;
}
```
<style>
    .container7 {
        display: grid;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element7 {
        width: 100px;
        height: 100px;
        justify-self: center;
        align-self: center;
        background-color: lightblue;
    }
</style>
<div class='container7'>
    <div class='element7'></div> 
</div>
<br>

**使用 grid 布局也是一种很优雅的方法**！可能唯一的不足就是浏览器兼容问题，不过目前绝大多数浏览器都能够兼容 grid 布局了，所以问题不大！

## 8、基于 flex 布局 

最后一个出场自然应该是压轴的重量角色！没错，所谓万能的**flex 布局**！

flex 布局强大的灵活性能够解决很多问题，也让很多原本复杂的布局问题都变得 so easy ！因此，我们只需要设置 `justify-content: center` 和 `align-items: center` 即可实现内部子元素的水平居中和垂直居中。代码如下：


```css
.container {
    display: flex; /* 关键代码 */
    justify-content: center; /* 关键代码 */
    align-items: center; /* 关键代码 */
    width: 200px;
    height: 200px;
    border: 2px solid black;
}
.element {
    width: 100px;
    height: 100px;
    background-color: lightgreen;
}
```
<style>
    .container8 {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 200px;
        border: 2px solid black;
    }
    .element8 {
        width: 100px;
        height: 100px;
        background-color: lightgreen;
    }
</style>
<div class='container8'>
    <div class='element8'></div> 
</div>
<br>

**没什么多说的，flex 布局 yyds ！**

# 总结

本文一共介绍了 8 种元素居中的方法，每个方法各有各的优点和缺点，可以根据实际情况按需使用。

---
**很感谢你能看到这里！谢谢～**

---
参考资料： (衷心感谢各参考资料提供的帮助)
+  [CSS实现水平垂直居中的10种方式](https://juejin.cn/post/6844903679242305544#heading-9)
+  [CSS深入理解vertical-align和line-height的基友关系](https://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/)
+  [line-height详解](https://blog.csdn.net/liting_1992/article/details/120888328)
+  [关于 vertical-align 你应该知道的一切](https://juejin.cn/post/6844904084885995528)
