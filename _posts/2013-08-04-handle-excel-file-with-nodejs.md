---
layout: post
title: "用NODEJS处理EXCEL文件"
description: "通过引用node-xlsx包，对xlsx文件进行读写，数据以JSON格式进行操作，包含从工作簿到工作表的描述。目前还不能处理xls文件。"
category: FE
tags: [js, nodejs, excel]
---
{% include JB/setup %}

有一些当时备份成 Excel 文件的旧数据，想把它们的数据结构调整一下，并对相关数据进行合并，以方便数据使用。这其实是之前一直想做的事，当时还在用 vs2008 写 c/s 程序什么的，简易上手的 C# 自己用的也还算熟络，可惜一直没做。再想起来时，电脑里早已没有了开发环境，写 C# 代码的信心也早没了。近期用 nodejs 写了一些本地小程序，读写文件也很方便用，于是寻思着用 nodejs 来处理。


###node-xlsx读取Excel文件

Excel 文件不能当普通的二进制文件处理，否则数据结构就被破坏了，于是需要先找个能够处理 Excel 文件的方法。搜索之下，[发现](http://cnodejs.org/topic/50b5a0b2637ffa415506a8c2#51fde51544e76d216ad97ec1)了 node-xlsx 包。由于也没发现其它可用的处理方式，于是先安装看看

<?prettify lang=bash?>
	npm i node-xlsx

查找 api 文档和示例代码未果，记得帖子里有说过，源码易懂，所以也翻了源码看看，发现两个接口 parse 和 build ，于是试着写了两行代码，居然成功了！

<?prettify lang=js linenums?>
	var xlsx = require('node-xlsx');

	// read
	var obj = xlsx.parse('a.xlsx');
	console.log(JSON.stringify(obj));

分析一下生成的 JSON 对象，把 Excel 文件描述的很实在，这样取数据或才设置数据的时候就很方便了。不过生成的数据中有太多字段，感觉是没用的。于是试着对它进行了精简，最终得到一个更直观的数据格式

<?prettify lang=js linenums?>
	{"worksheets":[{"data":[["索引1","索引2","c"]]}]}

直接对数据表与表内数据进行描述，在存储成 Excel文件的时候也没有问题

<?prettify lang=js linenums?>
	var xlsx = require('node-xlsx');
	var fs = require('fs');

	// write
	var obj = {"worksheets":[{"data":[["索引1","索引2","c"]]}]};
	var file = xlsx.build(obj);
	fs.writeFileSync('b.xlsx', file, 'binary');

一切进展的都很顺利。如果你也是，那么祝贺你，你的RP很高，关闭这个页面开干吧.

在最开始，我生成的 Excel 文件打开遇到了中文乱码的问题，很头疼。于是开始寻找解决乱码的方法。


###用csv格式保存数据表

另一位网友提出了[用 csv 格式保存 Excel 数据表](http://cnodejs.org/topic/516e0ab46d382773067a5473)的方案，我拿帖子回复里给出的代码也尝试了一下，发现确实也有 BOM 引起的乱码问题，可惜我没能把 BOM 加进去解决乱码，仍然是通过手动打开文件，修改 BOM 格式并保存后解决的。

当然，由于我的需求不是完全自动化处理这些数据，手动操作两下子也没问题。所以寻思着，用 node-xlsx 读取数据，调整数据结构，再保存为 csv 格式进行使用这个方案。由于前头到尾写的代码都在同一个 js 文件里，偶然间，再次打开用 node-xlsx 生成的 Excel 文件发现，居然没有乱码了！太奇怪了！恢复代码，再试一次，仍然不再乱码。

这算是灵异事件了吧，反正我不知道是怎么解决的，总之是在试过了 csv 保存，并手动调整了文件 BOM 后就不乱码了。它们之间有没有什么联系我也不知道，如果你跟我一样倒霉遇到同样的乱码问题，不妨按照这个流程试试，说不定就好了（xd）


###不能处理xls文件之痛

从头到尾的解决方案，都只能读取和保存 xlsx 格式的 Excel 文件，目前似乎也没有好的解决办法，反正我可不想重整 vs2008 环境用 C# 去写。由于需求的问题，文件格式的问题可以通过手动转换解决，于是我就这么做了。不管怎么说，数据的处理上能用程序处理，已经解决了我的主要问题了，非常感谢 node-xlsx 的贡献者！


