---
title: 基于 jsDeliver + Github 搭建免费图床教程
date: 2022-04-17 22:16:52
tags: 
     - jsDeliver
     - Github
categories: 教程 
comments: true
toc: true
top_img: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/background/astronaut3.png
cover: https://cdn.jsdelivr.net/gh/Douc1998/CDN-source/image/background/astronaut3.png
keywords: "jsDeliver, Github"
---

# 前言
之前在搭建博客的时候有想到要写这么一个教程，但是因为种种原因给搁置了。今天，我的好朋友突然跟我说：“你博客里的图片加载挺快的呀！” 我就想起来了这么件事，因此打算做一个简单的教程，也方便自己以后查看。

首先，或许大家看了我的[搭建博客教程](https://blog.douchen.life/%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/)或者网上的一些其他教程，已经成功搭建了属于自己的博客！

但是，因为我们的博客托管在免费的 [Github](https://github.com/) 平台上，所以访问和加载速度其实和服务器相比还是有点儿“感人”的。这时候，如果你**把博客里要用的一些图片或者资源都放在了本地并直接上传到博客对应的仓库中，则会影响到你博客的加载速度**。随着博客资源的增加，影响则会越发明显。

因此，我们可以通过 `创建图床` 的方式，来大大加速你的博客访问速度和图片资源加载速度。图床就是`存储图片的服务器`，即 **将你的图片上传到一个服务器上，然后通过特定的链接去访问**，十分好理解。

话不多说，下面我们就开始 `基于 jsDeliver + Github 搭建免费图床` ！

# 环境说明
+ **`jsDelivr`**
+ **`Github`**

这里就不详细说明 Github 是什么以及如何创建仓库，大家应该都很清楚了。不清楚的可以自行[百度](https://baidu.com)或者查看我[其他关于Github的教程](https://blog.douchen.life/%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B/)。 
## jsDelivr


