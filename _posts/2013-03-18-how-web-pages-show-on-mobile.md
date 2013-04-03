---
layout: post
title: "页面如何在手机上显示"
description: "本文通过对viewport的几个参数的效果进行对比加以解释，说明在各情况下页面是如何显示的"
category: FE
tags: [css, html, viewport]
---
{% include JB/setup %}

在 [让WebApp在iOS上模拟NativeApp的效果](/2012/08/29/webapp-on-ios/) 中提到了对页面进行一些 meta 标记的添加，实现类似 Native App 的表现，不过这种表现只是外在的，接下来看看页面内容的显示问题。

我们已知 viewport 在手机网页中有重要作用，但它具体起了哪些作用？

###viewport其事

viewport 直译为“视口”，它的作用可以这么想象：如同玩一个3D游戏，你能通过显示器看到3D场景，但一般场景大小远大于显示器屏幕大小，你需要移动和缩放场景之后，才能看到全部的场景。如果你用CSS写过3D的动画效果，联想一下 -webkit-perspective 属性:D

对，它就是手机上影响网页查看方式的神奇的“显示器”。它的尺寸决定你能看到的页面的尺寸，然后将 viewport 通过屏幕展示，你就可以看到页面内容了。

一般的桌面端网页都不会添加 viewport 设置，用智能手机查看这些页面时，是在设备的默认 viewport 设置下显示的。不同 [浏览器的默认 viewport](http://hax.iteye.com/blog/978184) 并不一致，比如 Apple Safari 默认是 980px，Android Webkit 默认是 800px，Windows Phone IE 默认是1024px，而桌面端页面最小宽度大概在800px左右。

![无viewport设置时查看页面方式示意图](/i/2013/03/18/10.gif "无viewport设置时查看页面方式示意图")

正如上图所示意的那样，一个大概 800px 宽的网页在手机上是通过如下的方式进行查看的：将网页放大，移动并查看。这种方式并不方便浏览，而一个针对手机开发的网页尺寸应当是适合屏幕显示，而不需要上下左右移动查看的，因此让页面能按最佳方式显示在手机上方便查看，正是设置 viewport 要做的事。

之前提到过，viewport 的设置方法是在页面的 head 区增加一个 name 值为 viewport 的 meta 标签，格式是

	<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">

content 值中填写各参数，其中 width 设置了 viewport 的宽度， device-width 的意思是设备屏幕的宽度，当然也可以设置为具体的数值，比如 640、320；initial-scale 设置了页面显示的初始缩放值；user-scalable=no 设置页面不能被缩放。viewport 更详细的信息可以参看 [Safari Developer Library 中的描述](https://developer.apple.com/library/safari/#documentation/appleapplications/reference/SafariHTMLRef/Articles/MetaTags.html)。

为了对 viewport 参数的属性值有直观的认识，这里借助 [Andreas Bovens](#0) 分享的关于 github 项目 [Understanding Viewport](https://github.com/andreasbovens/understanding-viewport) 的 [演示文稿](http://www.slideshare.net/andreasbovens/responsive-design-techniques-and-tricks-to-prepare-your-websites-for-the-multiscreen-future) 中第 30-63 页的几个截图说明一下。

这个 github 项目预先制作了 [几个 html 页面](http://andreasbovens.github.com/understanding-viewport/)，分别针对 viewport 的几个属性设置了特殊的值，直接在手机上查看这些页面可以看到页面的显示效果，并能测量当前页面的一些参数。请注意页面的背景图，包含一些格线及数字，它标识了一些常见的尺寸。

![项目页面截图](/i/2013/03/18/01.gif "项目页面截图")

如果页面没有设置 viewport 标签（情形1）：

![没有设置viewport时的截图](/i/2013/03/18/02.gif "没有设置viewport时的截图")

注：HVGA是指分辨率为 320\*480 的显示屏。

浏览器默认显示页面的全部内容，内容很小，无法正常浏览。正如截图中的那句话所说，是“980px 宽的页面被压缩到了 320px 宽显示”。980px 是默认宽度，此时页面 body 宽度为 980px，你看到的页面是 980px 宽，但由于屏幕宽度只有 320px ，所以 viewport 宽度缩小到了 320px ，看到的页面内容才会非常小。当然也可以通过缩放，将 viewport 的宽度放大到 980px 宽度，但这样就必须通过移动来查看屏幕以外的内容了。屏幕宽度`为何会是 320px 宽`（<a name="question" id="question">问题</a>），稍后再看。

这里你可能会疑惑：页面 body 的宽度怎样？

由于这些测试页面中的元素没有设置固定宽度，所以页面 body 的宽度是自适应的，viewport 有多宽，页面 body 就有多宽。而且，一旦页面加载完毕，页面 body 宽度就不再变化了（除非 viewport 的宽度改变），然而，viewport 的宽度改变仅仅可能发生在横竖屏切换的时候（排除js控制的情况），甚至缩放页面也不会改变页面的宽度。

只设置 viewport 的 width 属性为一个确切的尺寸（如 320 ）时（情形2）：

![设置viewport的width属性为320时的截图](/i/2013/03/18/03.gif "设置viewport的width属性为320时的截图")

切换横竖屏查看页面，页面的宽度均为 320px ，而且页面进行了自动缩放以适应屏幕。这种情况下，viewport 的宽度始终为 320px ，屏幕上也始终只能看到页面的 320px 宽的内容（虽然页面宽是适应的，但它本可以更多大）。

只设置 viewport 的 width 属性为 device-width 时（情形3）：

![设置viewport的width属性为device-width时的截图](/i/2013/03/18/04.gif "设置viewport的width属性为device-width时的截图")

竖屏查看页面时，页面宽度为 320px ，而横屏时，页面宽度变为 480px 。这种情况下，viewport 的宽度由设备的宽度决定，屏幕能看到页面的内容宽度竖屏时为 320px，横屏时为 480px。

对比以上两种情形的横屏显示：

![页面的缩放比率截图](/i/2013/03/18/05.gif "页面的缩放比率截图")

其实 viewport 能够自动调整的。

- 当 width=320 时，横屏后的 viewport 宽度仍然是 320px ，但可见区域宽度为 480px ，因此 viewport 要放大为 320 的 1.5 倍，把 320px 的页面显示到 480px 的屏幕上，从而页面显示的效果就被放大了。
- 而当 width=device-width 时，横屏后的 viewport 宽度变为了 480px ，此时的 viewport 没有缩放， initial-scale 值仍然为 1 ，所以页面没有经过缩放。

**viewport 自动调整特性：为了让页面适应 viewport 的显示区域进行显示，浏览器会自动根据已设置值的属性调整其它未设置值的属性值**。

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

但是，正因为这种自动调整的特性，有时候 viewport 的设置并不能按预期那样显示。比如当页面中包含一个宽度大于屏宽的元素时：

![设置viewport的width属性为device-width且元素大于屏宽时的截图](/i/2013/03/18/08.gif "设置viewport的width属性为device-width且元素大于屏宽时的截图")

用 Andreas Bovens 的 [测试页面](http://andreasbovens.github.com/understanding-viewport/meta-widthdevicewidth-div600.html) 似乎不能达到上图所示的结果。查看了样式后，发现页面中的 article 元素包含样式 overflow:hidden ，将这个样式定义去掉就会有图示的结果了。

按照理解，页面 body 宽度应该设置为 device-width 的值，一般来说是 320px ，从截图中蓝色区专块的尺寸可知，页面显示的尺寸应该为 320px，但 viewport 的宽度却不是 320px，就页面的显示结果来看，至少是大于 600px 了，viewport 的 initial-scale 值小于1，页面缩小了。但这个页面如果在一部 Android 手机上查看，可能会发现它显示的如下图一样：

![设置viewport的width属性为device-width、initial-scale属性为1且元素大于屏宽时的截图](/i/2013/03/18/09.gif "设置viewport的width属性为device-width、initial-scale属性为1且元素大于屏宽时的截图")

viewport 同时设置了 width 属性和 initial-scale 属性（情形7），在 iPhone 中显示的效果就如期望了。由于 viewport 的属性设置会相互影响，有时候简单的设置并不能如我们期望的那样显示（当然有的算是bug，有的也可以通过样式的方式来避免），但最好的办法就是按情形7那样，以更加保险的方式设置 viewport ，以保证页面按自己最初的意愿显示。



###屏宽320px的疑惑

回头看之前提到的 [问题](#question) ，为什么提到的屏宽都是 320px 的？我在 [这篇文章](http://www.iinterest.net/2011/05/02/about-viewport/) 中能找到了一个比较合理的说法：

	其实这个属性&值很有意思，字面意应该是 viewport 宽度等于设备宽度，但在实际中不同的浏览器都给出了个定值：320px；这个值还是源于 Apple ，因为早期 iPhone 的分辨率为 320px*480px ，大量为 iPhone 量身定制的网站都设置了 viewport:width=device-width ，并且按照宽度 320px 来设计制作，所以其他浏览器加入 viewport 支持时为了兼容性也将 device-width 定义为了 320px 。

所以就是个历史遗留问题了吧。

再从“技术角度”分析一下：早期的 iPhone 分辨率为 320px \* 480px ，也就意味着这块儿3.5吋屏**包含 320 \* 480 个物理像素**，按照计算机显示器的显示方式，这时候显示的页面宽度为 320px 。其它智能手机早期的分辨率基本上也使用了大致相同的屏幕尺寸与分辨率，因此才有了 320px 这个不约而同的约定。

iPhone 4 使用了IPS屏幕，这种屏幕其实也是一种液晶屏幕，与普通的液晶显示器差不多（不过还有许多优点），因此才有上面“包含 320 \* 480 个物理像素”的大胆猜想。这里提到的物理像素，可以和硬件设备的最小显示单元，也能称作“点”一一对应（但不总是这样）。关于这个问题，涉及到一些概念，有兴趣可以看看 [这篇文章](#TODO) 中对 Retina 显示屏的一些分析。



###最后

上述内容大多为自己的理解，如有不对的地方，还请指正，非常感谢！





