---
title: 基于React实现《推箱子》小游戏
date: 2022-04-28 10:04:09
tags: 
    - 前端
    - React
    - JavaScript
categories: React  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/background/Sokoban.png  # 文章页头部图片
cover: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/background/Sokoban.png  # 主页中显示的文章封面图片
keywords: "React，JavaScript，前端，推箱子"
---
# 前言
在写自己的网页的时候，突然想到写几个怀旧小游戏，比如贪吃蛇（已出教程）、推箱子（该教程）、俄罗斯方块等。上一篇文章已经讲解了[如何实现贪吃蛇小游戏](https://blog.douchen.life/React贪吃蛇小游戏/)，这里我将继续谈一谈如何基于 [React](https://zh-hans.reactjs.org/) 实现**推箱子**小游戏。主要内容依旧是这三项：
> 1. 静态设置
> 2. 游戏算法
> 3. React 渲染