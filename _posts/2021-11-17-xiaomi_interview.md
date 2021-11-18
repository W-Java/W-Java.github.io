---
layout: post
title:  "【面试总结】记录一次小米实习面试"
categories:  
tags: 总结
---

* content
{:toc}

# 前言  

笔者在11月15日接到了小米公司的来电，联系一面的时间，一个小时后就收到了邮件，时间定在了第二天下午的2点半

![img1](http://www.cywjw99.com/Xiaomi_interview/1.svg)

时间很紧迫，没什么时间复习了

记录一下都问了什么问题

# 面试问题

**面试官：可以做一个简短的自我介绍吗？**

我：balabalabala的介绍了大概一分钟自己的情况

**面试官：可以简单的介绍一下自己的项目吗？**

我：就说说项目背景、项目需求、个人职责、还有这个项目是怎么实现的等等

**面试官：可以说一下，其中一处你们完成的项目优化吗？**

我：有一个判断手机号的地方，可以同时判断出数据库中的数据上传情况，一举两得

**面试官：可以说一下，你另外一个Java项目吗？**

我：是一个java课程设计，用javaSwing实现UI界面，设置屏幕刷新率来实现动画效果，然后由于自己的UI实现的比较好，因此项目获得了小组最佳

**面试官：可以具体说一说，其中的动画效果是怎么实现的吗？**

我：通过设置屏幕1ms刷新率，来更新控件的坐标位置，实现对控件位置的实时更新，来让控件实现"运动"。然后通过对控件速度的设计，以及字母发射角度的控制，来让字母可以准确射击到单词的位置

**面试官：大致了解了，那你为什么选择应聘前端岗位？**

我：大二开始就对界面比较感兴趣，觉得前端的成就感不像后端，是肉眼可见的，因此就开始慢慢接触，随后就喜欢上了前端，然后学习之后发现，框架成熟，体系稳定，就很不错。。。

**面试官：ok那我们直接进入到前端知识的部分**

我：嗯嗯好

_开始了我的死亡时刻。。。_

**面试官：说一说css的样式选择器都有哪些？**

我：嗯....我记得有'.'和'#'分别代表了选择类（class）和选择id

**面试官：还有没有别的选择器了？**

我：嗯...暂时想不到了（感觉自己这个问题已经凉凉、确实没想到会问css,结果这么简单到东西也答不上来）

**面试官：你刚才说的是类选择器（.）和id选择器（#），还有最基本的标签选择器、派生选择器、组合选择器**

我：啊我去，我忘记说标签选择器了，因为那个是最基本的

**面试官：那你能说说，这几种选择器的优先级关系吗？**

我：嗯..!important>内联样式>ID>class>标签

**面试官：ok下一个问题**

[这个问题的详解](https://blog.csdn.net/qq_20179227/article/details/99705961?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163707984516780269852849%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163707984516780269852849&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-4-99705961.first_rank_v2_pc_rank_v29&utm_term=css%E7%9A%84%E6%A0%B7%E5%BC%8F%E9%80%89%E6%8B%A9%E5%99%A8&spm=1018.2226.3001.4187)

**面试官：现在有这样一个需求，让一个button无论在界面如何放大/缩小，都可以在界面的最中下部进行显示，如何用css进行实现？**

我：（这个问题，我觉得有很多种方法，当时有点紧张就只实现了几种不太好的）我想到了可以使用浮动、或者使用flex布局、或者使用calc()方法进行计算

**面试官：calc()并不是一个好的办法，这会无缘故的增加项目的负载**

我：哦（略显尴尬）没考虑这么多

**面试官：ok那我们来看看js的部分**

[这个问题的详解](https://blog.csdn.net/qq_42562636/article/details/99587838?ops_request_misc=&request_id=&biz_id=102&utm_term=%E8%AE%A9%E4%B8%80%E4%B8%AA%E6%8C%89%E9%92%AE%E5%A7%8B%E7%BB%88%E5%9C%A8%E7%95%8C%E9%9D%A2%E7%9A%84%E4%B8%AD%E4%B8%8B%E9%83%A8%E8%BF%9B%E8%A1%8C%E6%98%BE%E7%A4%BA&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-99587838.first_rank_v2_pc_rank_v29&spm=1018.2226.3001.4187)

_总结：我是真的没想到会问一些关于css的问题，我还以为会直接跳过html和css直接来进行js以及ES6的提问，这波属实是我大意了_

**面试官：能说说promise吗？**

我：这是一个ES6新增的语法，具体作用是balabala，但是我在项目中并没有实际使用过（感觉这个问题回答的也不是很如意）

[这个问题的详解](https://blog.csdn.net/qq_34645412/article/details/81170576?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163714902616780255233219%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163714902616780255233219&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-81170576.first_rank_v2_pc_rank_v29&utm_term=promise&spm=1018.2226.3001.4187)

**面试官：说说ES6的新特性吧？**

我：我知道的有箭头函数、let和const、for...of等等

[这个问题的详解](https://blog.csdn.net/bradmatt/article/details/80920153?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163716117116780255232064%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163716117116780255232064&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-80920153.first_rank_v2_pc_rank_v29&utm_term=ES6%E7%9A%84%E6%96%B0%E7%89%B9%E6%80%A7&spm=1018.2226.3001.4187)

**面试官：了解过行元素和块元素吗？**

我：嗯...听过，不过没有了解过（场面一度陷入到了尴尬的境地）不过这个貌似是html中的内容，不知道为什么放到了js的部分进行了考察

[这个问题的详解](https://blog.csdn.net/qq_42952262/article/details/103834029)

**面试官：说说箭头函数的特性吧？**

我：箭头函数就是ES6中的一个新的使用方法，他可以让函数的书写变得更简便一些

**面试官：这只是其中的最基本的一个，其实箭头函数还有很多特性。比如说：解决了JS中this的问题、没有arguments对象**

我：（其实我只知道this的那个，arguments的我真的不知道）

[这个问题的详解](https://blog.csdn.net/weixin_42554311/article/details/82589733?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163716197416780261928714%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163716197416780261928714&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-82589733.first_rank_v2_pc_rank_v29&utm_term=%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0%E7%9A%84%E4%BC%98%E7%82%B9&spm=1018.2226.3001.4187)

**面试官：说一说闭包吧？**

我：从什么是闭包，原理，解决的问题，以及存在的问题说了一遍

[这个问题的详解](https://blog.csdn.net/Hunt_bo/article/details/107699137?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163715667616780274194835%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163715667616780274194835&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-107699137.first_rank_v2_pc_rank_v29&utm_term=%E9%97%AD%E5%8C%85&spm=1018.2226.3001.4187)

**面试官：说一说js中改变this指向的函数都有哪些？**

我：call apply bind 其中call和apply的区别主要是传参的格式（call是一个一个传、apply则是用数组一整个传）bind是返回一个函数，进行二次调用

[这个问题的详解](https://blog.csdn.net/hexinyu_1022/article/details/82795517?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163716420616780264033229%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163716420616780264033229&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-82795517.first_rank_v2_pc_rank_v29&utm_term=call%2Capply%2Cbind&spm=1018.2226.3001.4187)

**面试官：我刚刚看了你的博客记载了关于AJax异步请求，可以详细说说吗？**

我：嗯嗯当时是因为在项目中遇到了关于axios相关的问题，所以就对ajax异步请求做了一个总结。然后我就大致对我对真实理解进行了一边解释，就是第一就是对原生的HTTPresquest进行封装，但是操作难度较高，不推荐新手使用，第二就是可以使用jquare，但是小题大做，我感觉没有必要，第三就是推荐的方法axios也是vue和react官方推荐使用的方法，不过axios并不是一种新的技术，本质上是对promise的封装，使用起来更加的简便

**面试官：我想问的是Ajax的原理你了解多少？**

我：我就只知道Ajax是一种异步的网络请求工具...

[这个问题的详解](https://blog.csdn.net/Oriental_/article/details/104863762?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163719409216780357248911%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163719409216780357248911&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-104863762.first_rank_v2_pc_rank_v29&utm_term=ajax&spm=1018.2226.3001.4187)

**面试官：对于get请求和post请求，有了解吗？**

我：我的了解不多，在项目中没有遇到需要我去甄别区别的地方....（场面异常尴尬）

[这个问题的详解](https://blog.csdn.net/csj731742019/article/details/108727469?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163721122616780357225177%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163721122616780357225177&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-4-108727469.first_rank_v2_pc_rank_v29&utm_term=get%E5%92%8Cpost%E7%9A%84%E5%8C%BA%E5%88%AB%E9%9D%A2%E8%AF%95&spm=1018.2226.3001.4187)

ps:这篇讲的真的是很形象易懂

**面试官：那我们来做两道笔试题吧**

**面试官：第一题就是一个js代码，问你会输出什么结果，答案是undefine因为那个函数内部的是局部变量，外层作用域中的变量虽然和内层的重名，但是并不是一个变量**

我：这题我回答错了...因为没有想到这个考点

**面试官：第二题也是一段js代码，也问会输出什么结果，答案也是undefine因为那个函数内部的this指向了window，并没有指向应该指向的变量**

我：答上了this但是没答上this指向了哪里（当时忘记了这种情况this应该指向window）

**面试官：那应该怎么改，可以避免这种出现undefine的情况呢？**

我：我觉得应该可以使用改变this指向的函数，比如说（bind、call、apply）

**面试官：其实最简单的方式就是把这个函数修改为箭头函数就行了....**

我：啊对啊.........（场面一度十分尴尬）

**面试官：今天的面试就到这里吧**

我：啊好.......面试官再见！辛苦啦

# 总结

人生第一次面试，以一个我认为还不错的开头，和一个不太好的结尾结束了

对我的意义大概就是，现在才真实的知道自己的菜，原来还是不那么愿意承认的emmm.....






