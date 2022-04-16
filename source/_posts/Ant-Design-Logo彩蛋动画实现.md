---
title: Ant Design Logo彩蛋动画实现
date: 2022-04-15 13:45:03
tags: 
    - 前端
    - CSS
categories: CSS  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/AntDesign.png  # 文章页头部图片
cover: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/AntdLogo.png  # 主页中显示的文章封面图片
keywords: "CSS，Animation"
---

# 前言
我们在做Web网页端项目的时候常常希望能让一些部件看起来更加美观，毕竟 **“爱美之心，人皆有之”**。然而，自己一个一个写 `CSS` 和 `JS` 实现又很麻烦。因此，现在出现了很多 UI 库，如 [Ant Design](https://ant.design/index-cn)、[Element UI](https://element.eleme.cn/) 等。这些库封装好了各式各样的网页部件，如 `Button`、`Icon`、`Menu`等，十分美观，我们可以直接拿来用。

有一天，打开 [Ant Design](https://ant.design/index-cn) 官网时，发现主页的 Logo 存在一个 **隐藏彩蛋** 👉 当我们把鼠标移入 Ant Design Logo上时，字母 `i` 上的图标会动态变化。

So，I have a dream ！ ...扯远了，So 我打算自己**重现彩蛋动画**。

# 实现原理
首先，我们实现的方法有两种:
+ 基于 `JavaScript` 脚本
+ 基于 `CSS` 动画

这里我们选择使用 **基于 `CSS` 动画** 实现该效果。

>为什么不使用 `JavaScript` 脚本呢？ 因为 `JavaScript` 实现动画的原理在于**为元素绑定事件函数**，实现多种动画样式，显然这种方式可以实现**复杂动画**且**自由度高**。
>
>然而也存在一些缺点，如 **代码量庞大**，以及**Js动画执行时是在主线程中运行，主线程中同时可能还有其他的Js脚本，可能会导致线程出现阻塞，加载慢，出现丢帧的情况**。

基于 `CSS` 动画实现的**思路**为：**将小图标在水平方向上拼接成长图，设置播放窗口大小为一个小图标的大小，并利用 `cover` 方式设置图片大小，以实现窗口只显示一个小图标，通过让小图标拼接图像左/右移动，实现图标切换播放**。

具体流程如下：
1. 准备好 Ant Design的 Logo 图标和各式各样的小图标拼接图。
2. 以Logo图标作为底，利用 **层叠样式的伪类元素 `::after`** 配合 **`CSS animation`** 实现小图标播放。 
3. 通过鼠标 `hover` 来控制动画的开始与暂停。

# 准备工作
我们需要准备两个图片素材，分别是 **Ant Design Logo 图标** 和 **Icons 拼接图**。
>感谢：这里直接引用iCSS前端趣闻公众号XboxYan博主的[素材](https://codepen.io/xboxyan/pen/LYOPoxz)。
## Ant Design Logo 图标
将 Ant Design Logo中字母 `i` 的一点扣掉，如下:

![Ant Design Logo](./Ant-Design-Logo彩蛋动画实现/logo.png)
## Icons 拼接图
为了避免多次请求，也为了方便创建动画，这里把所有小图标素材组合在一块（从官网另存下来的），如下:

![Icons 拼接图](./Ant-Design-Logo彩蛋动画实现/icons.png)

# 开始实现
实现流程主要分为两个步骤：  **`CSS` 动画设置** 和 **动画播放与暂停**。
## CSS 动画设置
首先，我们要在 `<style>` 标签中先定义 `.logo` 样式，将 Ant Design Logo作为我们的底图。

然后, 可变化的小图标用伪元素 `::after` 生成（装饰性的元素都可以用伪元素来生成，保证HTML的整洁），代码如下：
```CSS
<style>
  .logo{
    width: 500px;  
    height: 100px;
    position: relative;   
    color: transparent;
    background-image: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg'); 
    background-position: center;
    background-size: contain;
    background-repeat: no-repeat;
    /* 也可以统一，直接用 background写， 按照参数顺序即可 */
    /* background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg') center/contain no-repeat; */
    cursor: pointer;
  }
  .logo::after{
    content: '';
    position: absolute;
    width: 32px;
    height: 32px;
    background-image: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg'); 
    background-position: 0% 50%;
    background-size: cover;
    background-repeat: no-repeat;
    /* 也可以统一，直接用 background写， 按照参数顺序即可 */
    /* background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg') 0% 50% / cover no-repeat;*/
    right: 113px;
    top: -18px;
}
</style>
```
>上述代码，在伪类 `::after` 中，我们要定义窗口大小为 `height: 32px; width: 32px;` 以及设置图片大小为 `background-size: cover;` 从而只显示一个图标。
>
>接着利用 `right：113px； top: -18px;` 将窗口对应到字母 `i` 的上方。
>
>如果不了解 `background` 的相关参数的话，可以参考本人 Github 中的 CSS 笔记 👉 [设置背景图片](https://github.com/Douc1998/CSS-Notes/issues/3)。

静态效果如下：
<br>

<style>
  .logo0{
    width: 500px;  
    height: 100px;
    position: relative;   
    color: transparent;
    background-image: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg'); 
    background-position: center;
    background-size: contain;
    background-repeat: no-repeat;
    /* 也可以统一，直接用 background写， 按照参数顺序即可 */
    /* background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg') center/contain no-repeat; */
    cursor: pointer;
    text-align: center;
    margin: 0 auto;
  }
  .logo0::after{
    content: '';
    position: absolute;
    width: 32px;
    height: 32px;
    background-image: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg'); 
    background-position: 0% 50%;
    background-size: cover;
    background-repeat: no-repeat;
    /* 也可以统一，直接用 background写， 按照参数顺序即可 */
    /* background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg') 0% 50% / cover no-repeat;*/
    right: 113px;
    top: -18px;
}
</style>
<div class='logo0'></div>
<br>

我们刚刚在 **思路** 中提到，动画实现效果就是 **让条状长图在一个窗口上向左/右移动**。那么我们需要自定义一个 `move` 动画实现 `background-position` 的改变：
```CSS
/* 动画函数 -- background-position 在水平位置上移动到100% */
@keyframes move {
  to {
    background-position: 100%;
  }
}
```
接着，我们在伪类 `::after` 中定义动画即可。如下所示：
```CSS
.logo::after{
    /* 其他样式 */
    animation-name: move;
    animation-duration: 1s;
    animation-timing-function: steps(10, end);
    animation-iteration-count: infinite;
    /* 也可以统一，直接用 animation 写， 按照参数顺序即可 */
    /* animation: move 1s steps(10, end) infinite; */
}
```
>上述代码中，最重要的就是 **`steps(num, position)` 逐帧播放函数**，如有不了解，请移步 👉 [CSS动画steps功能](https://www.zhangxinxu.com/wordpress/2018/06/css3-animation-steps-step-start-end/)，当然也推荐你去查阅[官方文档](https://devdocs.io/css/animation-timing-function)。
>
>如果不了解 `animation` 的相关参数的话，可以参考本人 Github 中的 CSS 笔记 👉 [animation动画](https://github.com/Douc1998/CSS-Notes/issues/10)。

至此，我们的彩蛋复现已经完成了一大半了，效果如下：
<br>

<style>
.logo1{
  width: 500px;
  height: 100px;
  position: relative;
  color: transparent;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg') center/contain no-repeat;
  cursor: pointer;
  text-align: center;
  margin: 0 auto;
}
.logo1::after{
  content: '';
  position: absolute;
  width: 32px;
  height: 32px;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg') 0% 50% / cover no-repeat;
  right: 113px;
  top: -18px;
  animation: move 1s steps(10, end) infinite;
}
</style>
<div class="logo1"></div>

## 动画播放与暂停
从上面的动画效果可以看到，我们已经实现了图标的动态变换功能了。然而，距离完成还差一步：**通过鼠标的 `hover` 控制动画的播放与暂停**。

因此，我们需要在给 `.logo` 样式的伪类 `::after` 添上 `hover`属性。简单来说，思路就是：**没有hover就不播放动画，hover了之后添加并播放动画**。代码如下：
```CSS
<style>
.logo::after{
    /*
      其他样式
      区别：不包含 animation 属性
    */ 
}
.logo:hover::after{
    animation: move 1s steps(10, end) infinite;
}
</style>
```
效果如下：
<br>

<style>
.logo2{
  width: 500px;
  height: 100px;
  position: relative;
  color: transparent;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg') center/contain no-repeat;
  cursor: pointer;
  text-align: center;
  margin: 0 auto;
}
.logo2::after{
  content: '';
  position: absolute;
  width: 32px;
  height: 32px;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg') 0% 50% / cover no-repeat;
  right: 113px;
  top: -18px;
}
.logo2:hover::after{
    animation: move 1s steps(10, end) infinite;
}
</style>
<div class="logo2"></div>
<br> 

然而，虽然实现了 `hover` 控制播放的开始与暂停，但是我们发现：**当我们鼠标移开始之后，小图标又回到了点阵图。即每次都是重新创建动画，都是从头开始**。这不仅不满足我们的需求，而且多次创建动画还会影响渲染的性能。

**那我们该如何实现动画的暂停和继续，而不是每次重新开始呢？**

我们可以在伪类 `after` 中创建动画，并通过设置动画的状态 `animation-play-state` 来控制动画的暂停与开始。代码如下：
```CSS
<style>
.logo::after{
    /*其他样式*/ 
    animation: move 1s steps(10, end) infinite;
    animation-play-state: paused;
}
.logo:hover::after{
    animation-play-state: running;
}
</style>
```
**至此，就完成了我们所有的需求，结果如下：**
<br>

<style>
.logo{
  width: 500px;
  height: 100px;
  position: relative;
  color: transparent;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg') center/contain no-repeat;
  cursor: pointer;
  text-align: center;
  margin: 0 auto;
}
.logo::after{
  content: '';
  position: absolute;
  width: 32px;
  height: 32px;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg') 0% 50% / cover no-repeat;
  right: 113px;
  top: -18px;
  animation: move 1s steps(10, end) infinite;
  animation-play-state: paused;
}
.logo:hover::after{
  animation-play-state: running;
}
@keyframes move {
  to {
    background-position: 100%;
  }
}
</style>
<div class="logo"></div>

完整代码为：
```CSS
<style>
.logo{
  width: 500px;
  height: 100px;
  position: relative;
  color: transparent;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/logo.svg') center/contain no-repeat;
  cursor: pointer;
}
.logo::after{
  content: '';
  position: absolute;
  width: 32px;
  height: 32px;
  background: url('https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/icons.svg') 0% 50% / cover no-repeat;
  right: 113px;
  top: -18px;
  animation: move 1s steps(10, end) infinite;
  animation-play-state: paused;
}
.logo:hover::after{
  animation-play-state: running;
}
@keyframes move {
  to {
    background-position: 100%;
  }
}
</style>
```
```HTML
<div class="logo"></div>
```

---
**很感谢你能看到这里！谢谢～**

---
参考资料： (衷心感谢参考资料中各博主的帮助)
+  [Ant Design](https://ant.design/index-cn)
+  [Ant Design Logo](https://codepen.io/xboxyan/pen/LYOPoxz)  
+  [前端大全](https://mp.weixin.qq.com/s/wenkG3DjoPUkhO_wcnFBEw?v_p=90&WBAPIAnalysisOriUICodes=10000011_10000011&launchid=10000365--x&wm=3333_2001&aid=01A81Wr6YZ8BeQecF0jOz_QpTq0S8AtCl0HFWLx9flWD3AxNA.&from=10C4093010)

