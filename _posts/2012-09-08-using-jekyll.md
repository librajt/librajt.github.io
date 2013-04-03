---
layout: post
title: "WINDOWS下安装jekyll并在本地预览"
description: "Window环境下借助msysGit控制台安装jekyll，并配置运行本地服务器在本地预览github网站。"
category: SDE
tags: [ruby, jekyll]
---
{% include JB/setup %}

在 [搭建GitHub博客](/2012/08/26/blog-started) 的时候，就遇到了 jekyll 的安装问题，当时的解决办法是跳过，直接利用了 GitHub 对 jekyll 支持的特性，提交后在线上进行预览。

起初写写博客，倒还够用，最近想改模板了，总是进行 commit/push 操作怪麻烦的，加上偶尔网络一卡，一次修改便要了阵子才能看到效果。于是再次到虚拟机里尝试安装 jekyll 的命令，当然，报错依旧。不过当搜索关键词 `Failed to build gem native extension` 查找原因是，找到了 [这个博客](http://chxt6896.github.com) ，倒是有关于 jekyll 的文章。

[这篇文章](http://chxt6896.github.com/blog/2011/11/30/blog-jekyll-install.html) 提到了 windows 下安装 jekyll 需要注意的问题。虽然只有短短四步，但通读下来我也没能理解第一步到底是要做什么。继续往下看，提到 msysGit 下执行命令。嗯，在虚拟机里也装了 msysGit ，打开 msys.bat 进入控制台，输入命令 `gem install jekyll` 居然开始安装了，不再报错了！梳理下安装流程，回到本地环境准备搞起。

###实施过程

1 我使用的 git 客户端是 msysGit + TortoiseGit ，开始之前就已经安装好了。

2 [下载Ruby的window安装包](http://rubyinstaller.org/downloads/) 安装 Ruby 环境。

3 打开 msysGit 的 msys.bat 进入控制台，输入命令

	gem install jekyll

直到安装完毕。作为测试，可以重新打开一个 cmd 窗口，输入 

	jekyll --version

测试安装是否成功。

4 在启动服务器前，如果博文文件有 utf-8 编码的，需要打开 Ruby 安装目录下的 `lib\ruby\gems\1.9.1\gems\jekyll-0.11.2\lib\jekyll\convertible.rb` 文件（找不到可以搜索，版本号可能会有不同），把

	self.content = File.read(File.join(base, name))

替换成

	self.content = File.read(File.join(base, name), :encoding => "utf-8")

保证后续操作不会因编码问题影响生成本地网站。

5 在 cmd 中 cd 到 github 根目录，运行命令

	jekyll --server --auto

启动服务器，待屏幕不再滚动时，再到浏览器打开地址 `http://localhost:4000` 预览。

完美了！

jekyll 的噩梦终于结束了！


###后记

在之前近一个月的折腾中，不少文章提到还需要安装 Ruby Development Kit ，我安装过但是依然不能在 cmd 下安装 jekyll 。在本地环境安装时，只安装了 Ruby 就直接到 msysGit 里安装 jekyll 了，没有安装 Ruby Development Kit。





 


