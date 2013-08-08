---
layout: post
title: "使用weinre对WebApp页面dom进行远程调试"
description: "在nodejs环境下安装weinre对移动设备上的页面进行远程调试实战。"
category: SDE
tags: [node, weinre, webapp, 调试]
---
{% include JB/setup %}

做为一个 csser ，对页面调试的要求其实并不高，一般来说，只要对照视觉设计原稿，肉眼“调试”就成了。不过在页面完成交由后续开发的过程中（比如后端动态输出、引入 js 等），对一些一时不能确定问题所在时， csser 还是需要协助检查一下 dom 结构是不是按照设计之初正常工作的。

回想刚开始进入 webapp 领域，遇到一个了页面动画切换问题，无法判断 js 执行后是否生成了正确的 dom 结构。使用 css 背景色调试法简单得到了答案。作为确认，需要检查当前的 dom 结构，苦于手中无可用的调试工具，便请 jser 现场写了一段代码，将页面 html 代码输出到页面中进行排查。

当然，在桌面浏览器通过模拟 ua 进行页面调试，同样可以达到效果，但移动设备上的浏览器毕竟与桌面浏览器不同，对于无法在桌面浏览器复现的问题只能看着页面干着急了。

从csser的角度来讲，只需要确认 dom 结构正确就可以了，所以一般来说，浏览器模拟 ua 就能完成大部分工作。当下来讲， chrome 是个不错的选择。不过，毕竟是模拟，总有满足不了需求的时候，于是还是尝试了下远程调试工具 weinre 。

###关于

weinre 的工作原理是，通过本地服务器代理获取页面 dom 结构及样式，在服务器端 debug 工具查看并操作。它的 debug 工具界面与 chrome 的开发者工具很像，操作易上手。 weinre 有 java 版本，也有 nodejs 版本（不管哪个版本，都要装环境啊装环境 Orz... ）。

###安装weinre

如果你的机器跟我的一样，不知道在什么时候已经安装了 java 环境，可以看 [这篇文章](http://www.iinterest.net/2012/02/08/debugging-mobile-web-applications-with-the-weinre/) 运行一下 java 版。下面说下 nodejs 版，顺便安装一下 nodejs 。

当然，先要安装 nodejs ，到 [官网](http://nodejs.org/) 下载安装包，安装即可。 windows 用户可直接选择 .msi 格式安装包，一键安装，帮忙设置了环境变量，但安装目录不可选。如果要自定义，下载 .exe 格式可执行文件，放至自定义目录中，再到 [这里](http://nodejs.org/dist/npm/) 下载合适的 npm 包，解压后放在 nodejs 的自定义目录中，最后将目录路径添加到环境变量中，即可在 cmd 中使用 node 命令和 npm 命令了。设置环境变量后，需要重新打开 cmd 窗口。

安装 weinre ，在 cmd 中切换当前目录到 nodejs 目录，执行命令

<?prettify lang=bash linenums=true?>
	npm install weinre

等待安装完成。安装成功后，在 node_modults 目录下会生成一个 weinre 目录，进入该目录，输入命令

<?prettify lang=bash linenums=true?>
	node weinre --boundHost 192.168.137.78

启动服务器（本机ip），它会在本地的 8080 端口生成站点，在桌面浏览器访问 http://192.168.137.78:8080 会看到 weinre 相关信息。

要进行远程调试，需要向将要调试的页面注入一段 js 代码，比较好的方法是通过书签动态添加。在 weinre 相关信息页面上，有个叫 weinre target debug 的链接，将它添加到书签备用。在移动设备上，可以先随便添加一个书签，再通过书签的编辑功能，将 url 替换成信息页面中 pre 标签或 textarea 标签中的内容即可。

###使用

打开在信息页面上的第一个链接 debug client user interface 所指向的页面，即可看到调试工具。在移动设备上打开需要调试的页面，通过打开书签的方式，注入 js ，调试工具的 Targets 项中即有可调试的目标了。点击使它变成绿色，再点击上方控制面版中的 Emements 页，即可进行 dom 调试。

为方便使用，可以建立一个 bat 脚本，输入命令

<?prettify lang=bash linenums=true?>
	start http://192.168.137.78:8080
	node weinre --boundHost 192.168.137.78

并保存，在需要远程调试的时候，直接运行 bat 脚本就ok啦:D

win7 下脚本不在 weinre 目录下时，会因找不到文件报错，可以先先进入到该目录下再执行命令。修改 bat 脚本内容为：

<?prettify lang=bash linenums=true?>
	start http://192.168.137.78:8080
	c:
	cd\
	cd "Program Files\nodejs\node_modules\weinre"
	node weinre --boundHost 192.168.137.78

当然，这里的调试只提供了有限的功能，反正只是为了满足有限的需求嘛，将就着用吧。必要时，还是得靠桌面浏览器 + 模拟 ua 完成调试。

据说 chrome 支持远程调试，期待功能完善。

###扩展

关于调试的更多工具，可以查看 [这里](http://blog.csdn.net/slalx/article/details/7588940)

更详细的 weinre 介绍看  [这里](http://www.wybai.net/post/2011-11-09/20511143)









