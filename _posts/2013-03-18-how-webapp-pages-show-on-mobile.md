---
layout: post
title: "页面如何在手机上正常显示(草稿)"
description: "也许你已经知道viewport可以搞定，但那并不能搞定一切。关于页面构建与视觉设计，还有一些不得不说的事。"
category: 
tags: [webapp, css, html]
---
{% include JB/setup %}

在 [文](#0) 中提到了对页面进行一些 meta 标记的添加，实现类似 Native App 的表现。当然，这种表现只是外在的，接下来说一下页面内容的显示问题。

如果直接用 iPhone 等智能手机浏览桌面网站，会看到页面显示的很小，虽然可以通过缩放和移动查看页面，但桌面网页终究不适合在移动设备上浏览。我们已知 viewport 在手机网页中占重要地位，但它具体起了哪些作用？

###viewport其事

viewport 直译为“视口”，它的作用可以这么来想象一下：如同玩一个3D游戏，你能通过显示器看到3D场景，但一般场景大小远大于显示器屏幕大小，你需要移动和缩放场景之后，才能看到全部的场景。

对，它就是手机上用来查看网页的神奇的玩意儿。如果你用CSS写过3D的动画效果，再联想一下 -webkit-perspective 这个属性，你会对 viewport 的认识更深刻的:D

一般的桌面端网页都不会添加 viewport 设置，因此，用智能手机查看这些页面时，是在设备的默认 viewport 设置下显示的。不同 [浏览器的默认 viewport](http://hax.iteye.com/blog/978184) 并不一致，比如 Apple Safari 默认是 980px，Android Webkit 默认是 800px，Windows Phone IE 默认是1024px，而桌面端页面最小宽度大概在800px左右，因此默认能看到的大部分页面都全部显示在屏幕中而不用通过水平滑动查看屏幕外的其它部分。

![无viewport设置时查看页面方式示意图](/i/2013/03/18/10.gif "无viewport设置时查看页面方式示意图")

正如上图所示意的那样，一个大概 800px 宽的网页在手机上是通过如上的方式进行查看的。因此，对一个针对手机开发的网页来说，让页面能按最佳方式显示在手机上方便查看，正是设置 viewport 要做的事。

之前提到过，viewport 的设置方法是在页面的 head 区增加一个 name 值为 viewport 的 meta 标签，格式是

	<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">

content 值中填写各参数，其中 width 设置了 viewport 的宽度， device-width 的意思是设备屏幕的宽度，当然也可以设置为具体的数值，比如 640、320；initial-scale 设置了页面显示的初始缩放值；user-scalable=no 设置页面不能被缩放。viewport 更详细的信息可以参看 [Safari Developer Library 中的描述](https://developer.apple.com/library/safari/#documentation/appleapplications/reference/SafariHTMLRef/Articles/MetaTags.html)。

为了对 viewport 参数的属性值有直观的认识，这里借助 [Andreas Bovens](#0) 分享的关于 github 项目 [Understanding Viewport](https://github.com/andreasbovens/understanding-viewport) 的 [演示文稿](http://www.slideshare.net/andreasbovens/responsive-design-techniques-and-tricks-to-prepare-your-websites-for-the-multiscreen-future) 中第 30-63 页的几个截图说明一下。

这个 github 项目预先制作了 [几个 html 页面](http://andreasbovens.github.com/understanding-viewport/)，分别针对 viewport 的几个属性设置了特殊的值，直接在手机上查看这些页面可以看到页面的显示效果，并能测量当前页面的一些参数。请注意页面的背景图，包含一些格线及数字，它标识了一些常见的尺寸。

![项目页面截图](/i/2013/03/18/01.gif "项目页面截图")

如果页面没有设置 viewport 标签（情形1）：

![没有设置viewport时的截图](/i/2013/03/18/02.gif "没有设置viewport时的截图")

浏览器默认显示页面的全部内容，内容很小，无法正常浏览。正如截图中的那句话所说，是“980px 宽的页面被压缩到了 320px 宽显示”。980px 是默认宽度，至于`为何会是 320px 宽`（问题1），稍后再看。

只设置 viewport 的 width 属性为一个确切的尺寸（如 320 ）时（情形2）：

![设置viewport的width属性为320时的截图](/i/2013/03/18/03.gif "设置viewport的width属性为320时的截图")

切换横竖屏查看页面，页面的宽度均为 320px ，而且页面进行了自动缩放以适应屏幕。

只设置 viewport 的 width 属性为 device-width 时（情形3）：

![设置viewport的width属性为device-width时的截图](/i/2013/03/18/04.gif "设置viewport的width属性为device-width时的截图")

竖屏查看页面时，页面宽度为 320px ，而横屏时，页面宽度变为 480px 。

对比以上两种情形的横屏显示：

![页面的缩放比率截图](/i/2013/03/18/05.gif "页面的缩放比率截图")

其实 viewport 能够自动调整的。

- 当 width=320 时，横屏后的 viewport 宽度仍然是 320px ，但可见区域宽度为 480px ，因此 viewport 要放大为 320 的 1.5 倍，把 320px 的页面显示到 480px 的屏幕上，从而页面显示的效果就被放大了。
- 而当 width=device-width 时，横屏后的 viewport 宽度变为了 480px ，此时的 viewport 没有缩放， initial-scale 值仍然为 1 ，所以页面没有经过缩放。

**viewport 自动调整特性：为了将页面以“最佳”的方式显示在 viewport 中，浏览器会自动根据已设置值的属性调整其它未设置值的属性值。**。

	If you set only some of the properties, then Safari on iOS infers the values of the other properties with the goal of fitting the webpage in the visible area.

这些可相互影响的属性值主要包括：width、height、initial-scale。

只设置 viewport 的 initial-scale 属性为 1 时（情形4），得到与情形3同样的结果。

只设置 viewport 的 initial-scale 属性为 0.5 时（情形5）：

![设置viewport的initial-scale属性为0.5时的截图](/i/2013/03/18/06.gif "设置viewport的initial-scale属性为0.5时的截图")

横屏时， initial-scale 值为 0.5 ，为了适应屏宽 320px ，viewport 的宽度值就变成了 320px / 0.5 = 640px ，页面 body 宽也即 viewport 的宽度。相应地，竖屏则变为 480px / 0.5 = 960px 。

只设置 viewport 的 initial-scale 属性为 2 时（情形6）：

![设置viewport的initial-scale属性为2时的截图](/i/2013/03/18/07.gif "设置viewport的initial-scale属性为2时的截图")

横屏时， initial-scale 值为 2 ，为了适应屏宽 320px ，viewport 的宽度值就变成了 320px / 2 = 160px ，页面 body 宽也即 viewport 的宽度。相应地，竖屏则变为 480px / 2 = 240px 。

当然，只设置了 width=320 时，横屏时 viewport 的 initial-scale 值为 1.5 也是自动调整特性的一种表现。

这里你可能会疑惑：页面 body 的宽度是多少？

由于这些测试页面中的元素没有设置固定宽度，所以页面 body 的宽度是自适应的，viewport 有多宽，页面 body 就有多宽。而且，一旦页面加载完毕，页面 body 宽度就不再变化了（除非 viewport 的宽度改变），当然，viewport 的宽度改变仅仅可能发生在横竖屏切换的时候（排除js控制的情况），甚至缩放页面也不会改变页面的宽度。联想一下桌面上的网页，改变窗口大小时，页面的显示就相当于 width=device-width 时切换横竖屏的效果了。

至此，再理解一下 viewport 显示页面的原理：

**viewport 显示页面的原理：保持页面的缩放比率为 viewport 在当前方向上的尺寸改变的缩放比率，从而将页面以“最佳”的方式显示在 viewport 中**。

	Specifically, the goal of Safari on iOS is to fit the webpage in the visible area when completely zoomed out by maintaining a ratio equivalent to the ratio of the visible area in either orientation.

这里要注意的是， viewport 改变的是自身宽度，而不是页面的宽度。

有时候 viewport 的设置并不能按预期那样显示，比如当页面中包含一个宽度大于屏宽的元素时：

![设置viewport的width属性为device-width且元素大于屏宽时的截图](/i/2013/03/18/08.gif "设置viewport的width属性为device-width且元素大于屏宽时的截图")

用 Andreas Bovens 的 [测试页面](http://andreasbovens.github.com/understanding-viewport/meta-widthdevicewidth-div600.html) 似乎不能达到上图所示的结果。查看了样式后，发现页面中的 article 元素包含样式 overflow:hidden ，将这个样式定义去掉就会有图示的结果了。

按照理解，页面 body 宽度应该设置为 device-width 的值，一般来说是 320px ，从截图中蓝色区专块的尺寸可知，页面显示的尺寸应该为 320px，但 viewport 的宽度却不是 320px，就页面的显示结果来看，至少是大于 600px 了，viewport 的 initial-scale 值小于1，页面缩小了。但这个页面如果在一部 Android 手机上查看，可能会发现它显示的如下图一样：

![设置viewport的width属性为device-width、initial-scale属性为1且元素大于屏宽时的截图](/i/2013/03/18/09.gif "设置viewport的width属性为device-width、initial-scale属性为1且元素大于屏宽时的截图")

viewport 同时设置了 width 属性和 initial-scale 属性（情形7），在 iPhone 中显示的效果就如期望了。由于 viewport 的属性设置会相互影响，有时候简单的设置并不能如我们期望的那样显示（当然有的算是bug，有的也可以通过样式的方式来避免），但最好的办法就是按情形7那样，以更加保险的方式设置 viewport ，以保证页面按自己最初的意愿显示。



###320px的疑惑

回头看之前提到的 [问题1]() ，为什么提到的屏宽都是 320px 的？在 [这篇文章](http://www.iinterest.net/2011/05/02/about-viewport/) 中能找到一个比较合理的说法：

	其实这个属性&值很有意思，字面意应该是 viewport 宽度等于设备宽度，但在实际中不同的浏览器都给出了个定值：320px；这个值还是源于 Apple ，因为早期 iPhone 的分辨率为 320px*480px ，大量为 iPhone 量身定制的网站都设置了 viewport:width=device-width ，并且按照宽度 320px 来设计制作，所以其他浏览器加入 viewport 支持时为了兼容性也将 device-width 定义为了 320px 。

所以就是个历史遗留问题了吧。

那 device-width 就等于 320 吗？虽然可以这样理解，但那种说法却不是全对。

对于设备屏幕的分辨率，我们能很确定地从设备的参数列表中查到，比如 iPhone 4 的屏幕分辨率是 640px\*960px ，HTC G13 的屏幕分辨率是 320px\*480px 。那它们跟 device-width 有啥关系？














当然，这种情况并不是一成不变的，就目前来说， 320px 这个值已虽然是比较普遍的值，但也有一些特殊值，比如 360px（三星Note） 、400px（三星Note2）。





为了确定设备的 device-width 值，可以使用一个 [测试页面](http://librajt.github.com/WebAppHelper/package/PageRuler/) 通过 js 代码来动态获取。**注意：页面取出的屏幕分辨率在某些手机上并不准确，请以设备的参数列表中的数值为准**。


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







 


