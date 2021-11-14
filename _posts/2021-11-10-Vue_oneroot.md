---
layout: post
title:  "【个人总结】成功解决Component template should contain exactly one root element"
categories:  Vue
tags:  项目 bug 总结
---

* content
{:toc}


最近在信创前端项目组中

发现了这样一个bug

# 报错信息

![img1](http://www.cywjw99.com/img_vue_bug/1.svg)

> Component template should contain exactly one root element

# 错误翻译

对于这个bug，我们很容易翻译为：

**组件模板应该只包含一个根元素**

# 查看代码

我们找到对应bug的代码：

![img2](http://www.cywjw99.com/img_vue_bug/2.svg)

# 错误分析

根据Vue的单root模式可知，template下面包含了两个div根节点，这是报错的根本原因。

# 解决

在两个div之外包裹一个原生div即可，如图所示：

![img4](http://www.cywjw99.com/img_vue_bug/4.svg)

# 编译运行

![img3](http://www.cywjw99.com/img_vue_bug/3.svg)

运行成功！