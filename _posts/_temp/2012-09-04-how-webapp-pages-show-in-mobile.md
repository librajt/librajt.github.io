---
layout: post
title: "如何开发Mobile Page"
description: "也许你已经知道viewport可以搞定，但那并不能搞定一切。关于页面构建与视觉设计，还有一些不得不说的事。"
category: 
tags: [webapp, css, html]
---
{% include JB/setup %}


直接用 iPhone 等智能手机浏览桌面网站，会看到页面显示的很小，虽然可以通过缩放查看页面，但桌面网页终究不适合在移动设备上浏览。

###viewport其事

在移动设备中， viewport 在页面的显示中起到很大的作用。

一般的桌面端页面中都不会添加 viewport 设置，因此，用智能手机查看这些页面时，是在设备的默认 viewport 设置下显示的。不同浏览器的默认 viewport 并不一致，比如一些 [Apple Safari 默认是 980px，Android Webkit 默认是 800px，Windows Phone IE 默认是1024px](#hacker的页面)，而桌面端页面最小宽度大概在800px左右，因此默认能看到的大部分页面都全部显示在屏幕中而没有水平滚动条。

了解了 viewport 的作用，那么便可以想办法设置它的参数，以便页面正常显示。

viewport 的设置方法是，在页面的 head 区增加一个 name 值为 viewport 的 meta 标签，格式是

	<meta id="viewport" name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">

content 值中填写各参数，其中 width 设置了 viewport 的宽度， device-width 的意思是设备屏幕的宽度，当然也可以设置为具体的数值，比如 640、320；initial-scale 设置了页面显示的初始缩放值；user-scalable=no 设置页面不能被缩放。viewport 更详细的信息可以参看 [Safari Developer Library 中的描述](https://developer.apple.com/library/safari/#documentation/appleapplications/reference/SafariHTMLRef/Articles/MetaTags.html)。


###分辨率宽度与屏幕宽度

对于设备屏幕的分辨率，我们能很确定地从设备的参数列表中查到，比如 iPhone 4 的屏幕分辨率是 640px*960px ，HTC G13 的屏幕分辨率是 320px*480px 。

但是之前在 viewport 的设置中， width 应该设置为多少合适呢？上面的示例中，设置为 device-width ，它的意思是设备屏幕的宽度。那么设备屏幕的宽度是多少？

在 [这篇文章](http://www.iinterest.net/2011/05/02/about-viewport/) 中能找到一个比较合理的说法：

	其实这个属性&值很有意思，字面意应该是 viewport 宽度等于设备宽度，但在实际中不同的浏览器都给出了个定值：320px；这个值还是源于 Apple ，因为早期 iPhone 的分辨率为 320px*480px ，大量为 iPhone 量身定制的网站都设置了 viewport:width=device-width ，并且按照宽度 320px 来设计制作，所以其他浏览器加入 viewport 支持时为了兼容性也将 device-width 定义为了 320px 。

当然，这种情况并不是一成不变的，就目前来说， 320px 这个值已虽然是比较普遍的值，但也有一些特殊值，比如 360px（三星Note） 、400px（三星Note2）。

为了确定设备的 device-width 值，可以使用一个 [测试页面](http://librajt.github.com/WebAppHelper/package/PageRuler/) 通过 js 代码来动态获取。`注意：页面取出的屏幕分辨率在某些手机上并不准确，请以设备的参数列表中的数值为准。`

测试页面二维码：
<img src="http://www.qrcn.net/api?chs=150x150&chl=http://librajt.github.com/WebAppHelper/package/PageRuler/" alt="测试页面地址：http://librajt.github.com/WebAppHelper/package/PageRuler/">

###几个不得不说的概念

	1. 像素
	2. 屏幕尺寸：如 3寸屏（HTC G13）、3.5寸屏（iPhone 3GS/4/4S）
	3. 屏幕分辨率：如 320px*480px（HTC G13、iPhone 3GS）、640px*960px（iPhone 4/4S）
	4. 设备像素密度：device-pixel-ratio
	5. dpi、ppi

概念解释。。

PageRuler

提到retina。。


###retina。。

图片示意如何解决清晰度问题


###viewport特性

以自适应为原则，最优显示页面

###页而构建最佳实践

全兼容方案
页面宽度，less开发

对ui的要求，偶数


只面向ios
页面尺寸可以与ui一比一对应，配合viewport，完成细腻的ui，比如一像素边框


###关于wap2.0-的页面

另外一篇


学习总结，大量参考，有不正常的地方
引用







 


