---
layout: post
title: "Github博客诞生记：Ruby、jekyll统统玩儿蛋去"
description: ""
category: 
tags: [ruby, jekyll, github, blog]
---
{% include JB/setup %}

玩 GitHub 一段时间了，除了简单的提交代码来熟悉平台以外，基本上没有啥作为 ╮(╯▽╰)╭ 。

看到文章说用 GitHub Pages 可以搭建博客，嗯，至少找到一件事做。从看全英文的 GitHub Helper 页面，到 Google Baidu 各种教程，博客呱呱落地已是在开始研究后的两个星期了 Orz... 吐槽一下自己的英文水平，吐槽一下自己迟钝的变通能力，吐槽一下同质化严重以及质量偏低的网络教程， ect... 几乎要放弃了，终于在昨天结束的时候，柳暗花明。

玩 GitHub 博客，各种教程上都说需要安装 Ruby + jekyll ，向来不喜欢安装环境（悲催的 Windows 用户），于是先去虚拟机里搞。

Ruby 的安装比较顺利，有 Windows 下的一键安装包。按教程们说的安装 jekyll ，输入命令

	gem install jekyll

果不奇然，报错。但是错误信息里包含大量 linux 词汇，看过一遍不知所云，先按教程们说的，更新 Ruby 版本，输入命令

	gem update --system

运行良好。再次安装 jekyll ，无奈，错误信息是变了，但依然看不懂， Google Baidu 均无可喜进展，倒是又执行了不少命令安装了些东西。

教程们也说，可以用 rvm 安装 Ruby ，在这里其实 Ruby 环境已经安装完成了，不过病急乱投医，便去试了一试，无语的是，安装包都搞不定，最终做罢。

歇下先。

卷土重来时，再次被安装 jekyll 的命令打败，不过已经尝试用 rake 命令建立了一篇文章，想起来偶然热身的命令

	gem install rake

无心插柳。

三度重来，纠结于 jekyll 命令无法运行，于是想了想 rake 命令生成的 .md 文件是如何转换成 .html 文件的。开始怀疑 是 jekyll 的某个命令能够编译并生成 .html 文件，但 forke 了几个已有博客源码，发现并没有对应的 .html 文件。好嘛，这才注意到 GitHub 支持 jekyll ，教程们说的输入命令

	jekyll --server

只是为了让博客能在本地预览。

预览！！

瞬间顿悟！

马上把 jekyll bootstrap 的包 push 到 GitHub 上，整个世界都清静了 (乛乛) 。本机环境上没有装 Ruby ，建立文章直接用 Copy/Paste 已有文件的方式。

***

达成成就：

- GitHub 使用 Pages 需要到 Repo 的 Admin 页面 Options 的 GitHub Pages 开启功能。
- 拥有 username.github.com 这样的地址，只需要建立一个名为这个地址的 Repo 即可，直接在 master 分支上提交源码。
- 使用 Tortoise 的 PuTTY Key 生成器，直接 Load 由 msysgit 生成的公钥，直接保存一个私钥即可在 Pageant 里加载使用。在 GitHub 里提交的 SSH Key 内容为公钥里的内容。
- 使用 Tortoise 的操作代码库时，如果总是提示输入密码，确认在本地代码库的 Tortoise Setting 中 Git - > Remot 设置， url 为 git@ 开头的地址才可使用 SSH 方式，否则为 HTTP 方式。
- 








