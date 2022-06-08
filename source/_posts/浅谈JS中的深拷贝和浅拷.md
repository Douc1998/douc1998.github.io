---
title: 浅谈JavaScript中的深拷贝和浅拷贝
date: 2022-04-27 22:07:04
tags: 
    - 前端
    - JavaScript
categories: JavaScript  # 类别
comments: true  # 开启评论
toc: true  # 显示文字toc
top_img: https://fastly.jsdelivr.net/gh/Douc1998/CDN-source/image/background/scene13.jpeg  # 文章页头部图片
cover: https://fastly.jsdelivr.net/gh/Douc1998/CDN-source/image/background/scene13.jpeg  # 主页中显示的文章封面图片
keywords: "JavaScript, 深拷贝, 浅拷贝"
---
# 前言

之前一直觉得在**深拷贝**和**浅拷贝**上没遇到过什么问题，因此也没有太留意。前段时间在写项目的时候突然遇到了相关问题，因此决定还是简单地做个记录以备之后需要。

# 问题引入

直接进入正题，我们引入一个例子，这样将更有助于我们学习深/浅拷贝的区别。

**问题**：存在一个数组 `a`, 其内部存放的元素是 `point` 对象。 现在我要获取数组的最后一个元素并保存为 `b`, 然后利用循环将数组 `a` 中的每一个元素的值等于前一个元素的值, 最后 `b` 的值是否会改变? 如果改变, 又如何让它不变?

# 解决方案

## 浅拷贝

很显然, 如果直接用 `b = a[a.length - 1]` 这种赋值方式时，`b` 和 `a[a.length - 1]` 都存储在**栈内存**中。**它们都只是对应堆内存中的 `point` 元素的引用**，指向一个**地址**。（并没有在堆内存中创建新的 `point` 元素）

当对 `point` 对象的值进行修改时，会发现 `b` 对应的值也会修改，这就是 **浅拷贝**。

```JS
// 定义 point 对象
class point{
    constructor(r = 0, c = 0){
        this.r = r;
        this.c = c;
    }
}

// 定义 a 数组
const a = [new point(1, 1), new point(2, 2), new point(3, 3)]
// 浅拷贝
const b = a[a.length - 1]
// 对 a 中的值进行修改
for(let i = a.length - 1; i > 0; i--){
    a[i].r = a[i - 1].r;
    a[i].c = a[i - 1].c;
}

console.log(a) // [ point { r: 1, c: 1 }, point { r: 1, c: 1 }, point { r: 2, c: 2 } ]
console.log(b) // point { r: 2, c: 2 } 也被修改了!
```

那么如何解决这种问题并且实现我们想要的结果呢？👉 **深拷贝**

## 深拷贝

深拷贝和浅拷贝的主要区别就在于：如何应对**复杂的Object对象**。

🌟 **深拷贝会在堆内存中新建一个同样的对象, 并在栈内存中创建指向该对象的引用**, 即 `b`。

```JS
import _ from 'lodash'; // 借助 lodash 实现深拷贝

// ...上一部分的对象定义和赋值等内容...

// 深拷贝
const b_cloneDeep = _.cloneDeep(a[a.length - 1])

console.log(b_cloneDeep) // point { r: 3, c: 3 }
```

我们也可以利用 `===` 来判断一下 深浅拷贝和原值是否完全相等。
```JS
console.log(b === a[a.length - 1])  // true 浅拷贝
console.log(b_cloneDeep === a[a.length - 1])  // false 深拷贝
```

# 总结
由上面的实例，我们大致地了解了深/浅拷贝的区别。虽然浅拷贝无法解决像上述的问题，但是在绝大多数情况下，**我还是建议使用浅拷贝**，因为浅拷贝可以满足我们绝大多数需求。

如果不是遇到必须使用深拷贝的情况，请不要随意使用深拷贝。因为深拷贝的**深度复制容易带来性能问题**，尤其是**当复制的对象层级复杂**时。

因此，要根据实际情况**合理使用深/浅拷贝**。

---
**很感谢你能看到这里！谢谢～**

---
参考资料： (衷心感谢各参考资料提供的帮助)
+  [js浅拷贝与深拷贝的区别和实现方式](https://www。jianshu。com/p/1c142ec2ca45)

