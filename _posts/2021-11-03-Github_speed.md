---
layout: post
title:  "【工具推荐】Github各种加速怎么实现"
categories: 
tags:  工具 Github
---

* content
{:toc}

分享一个笔者日常使用的Github加速器，StackOverflow同样适用，还支持控制台代理，轻量级亲测稳定好用

下载地址：[Pigcha](https://github.com/pigpigchacha/PigchaVPN)

当然也有一些不用科学上网的方法，这里也分享踩过的坑

## Github怎么加速？

clone速度缓慢？代码push不上去？Github登不上去？

先来说说几种解决的办法

### 笔者试过的

#### （一）配置host文件

1.打开[查询域名映射关系的工具](http://tool.chinaz.com/dns)

2.查询以下地址，得到其ip：

> github.global.ssl.fastly.net
>
> assets-cdn.github.com
>
> documentcloud.github.com
>
> github.com
>
> gist.github.com
>
> help.github.com
>
> nodeload.github.com
>
> raw.github.com
>
> status.github.com
>
> training.github.com
>
> www.github.com
>
> avatars0.githubusercontent.com
>
> avatars1.githubusercontent.com
>
> codeload.github.com


3.选择一个稳定，延迟较低的ip添加到host文件

我添加如下：

> 151.101.73.194 github.global.ssl.fastly.net
> 
> 151.101.72.133 assets-cdn.github.com
> 
> 13.250.177.223 github.com


**以前这种方式可以实现，不过现在已经失效了（至少我是没感觉到什么明显的变化）**

#### （二）使用镜像网站访问

最常用的两个

* https://github.com.cnpmjs.org

* https://hub.fastgit.org

**clone,push等操作没啥问题，但是毕竟mirror，味不纯了懂得都懂**

#### （三）通过Gitee中转fork仓库下载

这个很简单，从gitee上直接将github上的项目导入，然后通过gitee作为中转进行操作

**个人觉得麻烦，感觉很累赘**



### 笔者听过的

#### 用proxychains透明代理

原理是用git内置代理，直接走系统中运行的代理工具中转

设置git config

还要停用代理，感觉很麻烦


## 一劳永逸选择Pigcha

下载地址打在文章最开始，个人觉得超级方便github的使用者
