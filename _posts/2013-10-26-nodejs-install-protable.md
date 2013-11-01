---
layout: post
title: "nodejs在Windows中的绿色安装和使用"
description: "在Win7上安装nodejs和相关modules时，安装完毕却总会出现找不到modules的问题，除了环境变量配置的问题外，还有modules的安装目录问题。"
category: SDE
tags: [nodejs, mklink]
---
{% include JB/setup %}

在文章[《使用weinre对WebApp页面dom进行远程调试》](/2012/08/28/debug-with-weinre)中已经简单说明过安装 nodejs 的过程了，个我比较喜欢绿色的安装方式，所以在此再将安装过程列一遍。

###安装nodejs

1. 到 [官网](http://nodejs.org/) 下载 .exe 格式可执行文件，放在自定义的 nodejs 安装目录中。
2. 到 [这里](http://nodejs.org/dist/npm/) 下载合适的 npm 包，解压后放在 nodejs 的自定义目录中。
3. 将 nodejs 安装目录、目录中的 node_modules 目录路径，填入系统的环境变量中。

据说新版本的 .msi 安装包，除了可以一键安装，帮忙设置了环境变量外，安装目录也可选了，懒人倒可以试试。

安装完注意，目录应该是这样的：

	nodejs              // 根目录
	└─node_modules    // 子目录1
	    └─npm         // 子目录2
	    node.exe        // 根文件1
	    npm             // 根文件2
	    npm.cmd         // 根文件3

根文件1是 nodejs 的主文件，根文件2文件是在 bash 命令行中执行 npm 命令时将实际执行的文件，而根文件3是在 cmd 命令行中执行 npm 命令时将实际执行的文件。如果哪种情况下执行 npm 命令提示“npm commond not found”之类的错误信息时，就需要检查是否有对应的文件。


###安装modules

安装 modules 很方便，只要交给 npm 就可以了。比如要安装 bower ，打开命令行，`进入到 nodejs 安装目录`，在命令行中输入：

	npm install bower  // 或者可以简写为 npm i bower

在联网状态下，等待程序自动下载并安装，即可完成安装。

完成安装后，在根目录下会生成同 npm 类似的根文件们和子目录，一般情况下没有问题，如果有对应的 node modules 安装成功后，却提示“命令找不到”时，检查是否有对应的文件就可以了。

在安装 modules 时， npm 的参数中可以跟 -g 以全局安装，`在此我们不需要`，否则可能会把文件下载并安装到 

	C:\Users\<name>\AppData\Roaming\npm

中去。


###用Win7的mklink命令将绿色进行到底

window 系统中的一些 modules 在使用时，需要在执行 nodejs 命令所在的路径上能够找到并使用对应 modules 的文件，一般的做法是将 nodejs 根目录下的 node_modules 目录复制到相应的开发根目录。

这样做有个麻烦之处，是每新增一个 modules 时，如果它需要这样的操作，就需要手动复制一次。

在 Win7 的命令行中，有个 mklink 命令，如如需要执行上面的操作，可使用该命令对 nodejs 根目录下的 node_modules 目录生成一个“快捷方式”：

	mklink /d "E:\Work\node_modules" "D:\Program Files\nodejs\node_modules"

这样就可以保持两个目录下的 node_modules 目录内容保持一致，安装新的 nodules 时，任意文件夹都可以。

事实上，新建的“快捷方式”是完全指向源目录的，虽然可以以新的路径查看源路径的文件，但实际文件还是存在在源目录中。关于这个命令的详细说明与使用测试，可以看[这里](http://www.cnblogs.com/asion/archive/2011/03/10/1979282.html)。

这样一来，nodejs 的安装目录在系统分区以外了，而且“快捷方式”也能统一快速生成了，如果不幸系统重装，这些 nodejs 的 modules 都还在，只需要在新系统中重新添加环境变量， nodejs 就可以绿色安装完毕了:D



