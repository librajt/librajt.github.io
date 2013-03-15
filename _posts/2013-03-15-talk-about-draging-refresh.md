---
layout: post
title: "关于下拉刷新的交互体验"
description: "常见的下拉刷新、上拉加载更多这样的交互体验，给用户反馈的结果与指引的一些思考"
category: 
tags: [ue, app, webapp, stream]
---
{% include JB/setup %}


下拉刷新一般有三种状态：

	状态A：提示“下拉可以刷新”
	状态B：达到刷新距离，提示“松手即可刷新”
	状态C：加载更新

这种交互方式如今随处可见，可是偶然间发现，一般对应三种状态包含的箭头 icon ，不同的应用中也不尽相同。

比如 [网易新闻客户端](http://itunes.apple.com/cn/app/id425349261?mt=8) 的下拉刷新功能表现（方案1）（以下各种 app 均指 iOS 上的版本）：

<img src="/i/2013/03/15/01.jpg" alt="网易新闻客户端截图" title="网易新闻客户端截图">

它的状态A中，箭头是向下的。如果在状态变化之前松手，箭头不会变化方向。
变成状态B时，箭头方向变为了向上，持续向下拉直到松手，它会变成状态C。如果中途反向拉，直到它变回状态A时，便不会响应刷新功能。

这是大多数 app 的下拉刷新功能表现，有一段时间，我觉得这种箭头的变化有点奇怪：为何从状态A变到状态B时，箭头反向了，而状态B的箭头是向上的，而文字说明却是“松手即可刷新”？此时是在告诉用户什么信息，是箭头说的“向上”，还是文字说明的“松手”？


那么是翻译的问题吗？墙内用户表示，无法看 [鼻祖Twitter](http://www.36kr.com/p/94602.html) 客户端的下拉刷新表现，那就看看国外应用 [AppFlow](http://itunes.apple.com/hu/app/id539183053?mt=8) 的下拉刷新英文版吧：

<img src="/i/2013/03/15/02.jpg" alt="AppFlow客户端截图" title="AppFlow客户端截图">


觉得迷惑，也可能只是钻牛角尖了吧，但在 [搜狐微博触屏版](http://w.sohu.com/m) 中，我们把状态A与状态B的箭头动态效果给去除了（方案2）：

<img src="/i/2013/03/15/03.jpg" alt="搜狐微博触屏版截图" title="搜狐微博触屏版截图">

虽然同时也为了回避一个技术问题，哈哈哈！


在最新版的 [Gmail移动版](http://mail.google/) 中，它的下拉刷新也有箭头，但与前面说的箭头方向是相反的（方案3）：

<img src="/i/2013/03/15/04.jpg" alt="Gmail移动版截图" title="Gmail移动版截图">

与方案1不同的是，它只用了箭头表示，但并未使用文字说明。杜撰了一下各状态箭头所代表的含义，感觉还挺有意思（个人想法）：

	状态A：箭头向上。可能在说“不要向下拉了，松手吧”
	状态B：箭头变向下。可能在说“好吧，继续向下拉吧，我不会阻止你了。”
	如果箭头变为向下后开始反向，箭头会恢复向上。这时可能在说“对，继续继续”
	状态C：一个四色翻折的圆形纸板，看起来翻的很纠结的样子

经过以上的理解， Gmail 的下拉刷新功能，似乎在向用户传达“轻易不要刷，浪费请求浪费流量，哪有那么多新邮件”的意思（无良YY）。反过来再看最开始提到的下拉刷新功能表现，想单纯通过箭头来理解功能的意向，却免不了联想到文字传达的意思，着实迷惑。


对比以上三种方案：

	方案1：中规中矩，司空见惯，即使有表意冲突可能让人迷惑，但也不至于引起用户使用上的障碍。
	方案2：方案1的改良，只使用文字，相对更直观明了。
	方案3：呃。。想不到词了，工程师很调皮。但简单的功能表现，却更容易吸引用户注意力！

个人觉得，方案2相对来说表意更明确一些，但方案3更有意思。


另外，做为 iOS 的轻度用户，发现 iOS6 中的邮件客户端中，邮件列表的下拉刷新功能表现挺有意思：

<img src="/i/2013/03/15/05.jpg" alt="iOS6邮件客户端截图" title="iOS6邮件客户端截图">

始终是一个刷新按钮，随着拉动距离的变化，按钮大小会有变化，而且按钮的外观会被拉长变形，这种表现对用户继续下拉以执行刷新操作，实在是一种不小的刺激。


类似的效果在 [Path](http://itunes.apple.com/cn/app/path/id403639508?mt=8) 的3.0版本中，也得到同样的体现：

<img src="/i/2013/03/15/06.jpg" alt="Path客户端截图" title="Path客户端截图">

之前的刷新icon变成了一个小圆圈，下拉的时候会变成倒着的小水滴形状，最终弹性弹落到timeline上方，很有意思。


[微信](http://itunes.apple.com/cn/app/id414478124?mt=8&ls=1) 朋友圉的下拉刷新则更加简单：

<img src="/i/2013/03/15/07.jpg" alt="微信客户端截图" title="微信客户端截图">

只有一个彩色的轮子做变化。


作为一个前端工程师，思维方式总有奇怪之处，个人对于下拉刷新功能表现的纠结与情结，最终倾注在了 [手机搜狐触屏版](http://h5.m.sohu.com) 中，不过不是“下拉刷新”，而是“上拉加载更多”：

<img src="/i/2013/03/15/08.jpg" alt="手机搜狐触屏版截图" title="手机搜狐触屏版截图">

与方案1相同，仍然有箭头变化，但通过文案来吸引用户。“放开它放开它”，好吧，卖萌吧:D


