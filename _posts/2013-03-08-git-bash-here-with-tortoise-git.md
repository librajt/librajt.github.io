---
layout: post
title: "添加Git Bash Here右键菜单"
description: "Tortoise Git的Window客户端安装后没有实用的Bash Here快捷菜单，但又不习惯 Git for Windows 的GUI版，那就手动移植过来吧"
category: 
tags: [git, bash, TortoiseGit, msysGit]
---
{% include JB/setup %}

Git Bash Here 是 Git for Windows 的右键快捷菜单项，它与 “DOS Here” 作用打开一个 cmd 窗口一样，将当前文件或目录的路径在 git command 窗口中打开。对于鼠标流来说，这个快捷菜单可是省事不少。

Git for Windows 包含GUI界面，但既然已经习惯了 Tortoise Git 操作界面，又不愿意去更换工具，想使用 Bash Here 功能，就只有自己造菜单了。

参考了 [这篇文章](http://blog.sina.com.cn/s/blog_4dbcd2730100zxt7.html) 后，先是到虚拟机里安装了 Git for Windows ，然后循址取到了关键的 Git Bash.vbs 文件，内容如下：

	Set fso = CreateObject("Scripting.FileSystemObject")
	Set shell = CreateObject("WScript.Shell")

	Const TemporaryFolder = 2
	linkfile = fso.BuildPath(fso.GetSpecialFolder(TemporaryFolder), "Git Bash.lnk")
	gitdir = fso.GetParentFolderName(WScript.ScriptFullName)

	' Dynamically create a shortcut with the current directory as the working directory.
	Set link = shell.CreateShortcut(linkfile)
	link.TargetPath = fso.BuildPath(gitdir, "bin\sh.exe")
	link.Arguments = "--login -i"
	link.IconLocation = fso.BuildPath(gitdir, "etc\git.ico")
	If WScript.Arguments.Length > 0 Then link.WorkingDirectory = WScript.Arguments(0)
	link.Save

	Set app = CreateObject("Shell.Application")
	app.ShellExecute linkfile


比较奇怪的是，在 msysGit 的 share 目录下也有一个同名文件，但不能用。

将上面的 .vbs 文件存放到了 msysGit 根目录下之后，造了下面这个 .reg 注册表文件：

	Windows Registry Editor Version 5.00

	[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\*\shell\git_shell]
	@="Git Ba&sh Here"
	[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\*\shell\git_shell\command]
	@="wscript \"C:\\msysGit\\msysgit\\Git Bash.vbs\" \"%1\""

	[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\git_shell]
	@="Git Ba&sh Here"
	[HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Directory\shell\git_shell\command]
	@="wscript \"C:\\msysGit\\msysgit\\Git Bash.vbs\" \"%1\""

注意路径及其中的斜杠转意符。

双击运行即可添加（XP下测试可用）。


##后记

升级 Tortoise Git 和 msysGit 最新版时，遇到一些安装包的选择问题，在这里记录一下：

1. 安装时提示“无法通过Windows Installer服务安装此安装程序包”时，需要更新系统补丁： [下载页面](http://www.microsoft.com/zh-cn/download/details.aspx?id=8483) 中最后一个下载安装即可（XP下）

2. 完整的 msysGit 本地安装包，可以到 [msysGit官方网站](http://msysgit.github.com/) 点击右面的下载入口去到下载页。但它默认提供的是在线安装版本，查看其它版本可以消除上方的过滤器内容，或者直接 [查看fullinstall版本](https://code.google.com/p/msysgit/downloads/list?can=2&q=fullinstall&colspec=Filename+Summary+Uploaded+ReleaseDate+Size+DownloadCount) 

3. 升级安装时，本想覆盖安装，但由于选错了路径，出现了新旧两个版本。后来索性先删除旧版本，重新安装到了默认系统根目录中

4. 升级完 Tortoise Git 到最新版后，发现旧版本的 Tortoise SVN 右键菜单的图标出现了位置偏差，不知啥原因，顺便直接也升级了 Tortoise SVN 的 [最新版本](http://tortoisesvn.net/downloads.html) ，问题解决




