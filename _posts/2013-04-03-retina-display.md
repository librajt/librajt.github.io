---
layout: post
title: "Retina显示原理探索"
description: "本文通过对viewport的几个参数的效果进行对比加以解释，说明在各情况下页面是如何显示的"
category: fe
tags: [css, html, viewport]
---
{% include JB/setup %}


###所谓Retina

是一种显示技术，可以将把更多的像素点压缩至一块屏幕里，从而达到更高的分辨率并提高屏幕显示的细腻程度，这种分辨率在正常观看距离下足以使人肉眼无法分辨其中的单独像素。


###Retina显示屏

最初采用该种屏幕的产品 iPhone 4 屏幕分辨率为960 \* 640（326ppi）。

对比如下两幅图，可以清晰地看出是否 Retina 屏的显示差异（图片来自 [维基百科](http://zh.wikipedia.org/wiki/%E8%A7%86%E7%BD%91%E8%86%9C%E6%98%BE%E7%A4%BA%E5%99%A8)）：

![iPhone3GS屏幕截图](/i/2013/04/03/1.jpg "iPhone3GS屏幕截图")

![iPhone4屏幕截图](/i/2013/04/03/2.jpg "iPhone4屏幕截图")

上图为 iPhone 3GS 屏幕截图，下图为 iPhone 4 屏幕截图。

两代 iPhone 屏幕上显示的内容是一样多的，但 iPhone 4 显示的画面更精细。

两代 iPhone 屏幕尺寸相同，都是 3.5 吋，但 iPhone 4 分辨率却是 iPhone 3GS 的4倍，

对比 iPhone 3GS，虽然 iPhone 4 分辨率提高了，但它不同于普通的电脑显示器那样为了显示更多的内容，而是提升显示相同内容时的画面精细程度。这种提升方式是靠提升单位面积屏幕的像素数量，即像素密度来提升分辨率，这样做的主要目的是为了提高屏幕显示画面的精细程度。


###Retina工作方式

以 [第三代 MacBook Pro with Retina Display](http://zh.wikipedia.org/wiki/MacBook_Pro#.E7.AC.AC.E4.B8.89.E4.BB.A3.E4.BA.A7.E5.93.81.28With_Retina_Display.29) 为例，工作时显卡渲染出的2880x1880个**像素每四个一组**，输出**原来屏幕的一个像素**显示的大小区域内的图像。这样一来，用户所看到的图标与文字的大小与原来的1440x900分辨率显示屏相同，但精细度是原来的4倍。

在桌面显示器中，我们调整了显示分辨率，比如从 800\*600 调整到 1024 \* 768 时，屏幕的文字图标会变小，显示的内容更多了。但 Retina 显示方式不会产生这样的问题，或者说，** Retina 显示技术解决的是显示画面精细程度的问题，而不是解决显示内容容量的问题**。


###设备独立像素

为什么是“每四个一组”？而且要让这四个一组来显示“原来屏幕的一个像素”？这大概就是 Retina 显示技术的一种表现吧。而这“每四个一组”的像素，可以被称作“设备独立像素”， device independent pixel ，或者 density-independent pixel ，它可以是系统中的一个点，这个点代表一个可以由程序使用的虚拟像素，然后由相关系统转换为物理像素。

	A Device independent pixel (also: density-independent pixel) is a point in a co-ordinate system held by a computer and represents an abstraction of a pixel for use by an application that an underlying system then converts to physical pixels.

“设备独立像素”也有人称为“CSS像素”，一种形象的说法，更倾向于表明与 CSS 中尺寸的对应。

设备独立像素与物理像素的对应关系，可以这样看：

![设备独立像素与物理像素的对应关系示意图](/i/2013/04/03/3.jpg "设备独立像素与物理像素的对应关系示意图")

类似的每四个一组的对应关系，也许正是 Retina 显示技术所做的。


###设备像素比

设备像素比定义了设备独立像素和物理像素的对应关系。它的值可以按如下的公式计算：

	设备像素比 = 物理像素 / 设备独立像素

设备像素比在 js 中可以通过 devicePixelRatio 的参数取得（需要页面的 viewport 设置为 content="width=device-width" ）

iPhone 4 的设备像素比为2，线长（横向、纵向、对角线）上的物理像素数与设备独立像素数的对应关系即为2。

根据这个对应关系，一般可以通过屏幕的物理分辨率和设备像素比确定设备独立像素数。


###再说屏宽320px的疑惑

[之前提到](/2013/03/18/how-web-pages-show-on-mobile) 屏宽320px基本上属于历史遗留问题，这里提到的屏宽，更确切地说，应当是 viewport 设置为 width=device-width 时的宽度，而我习惯称这个宽度为屏宽，其实也就是设备独立像素的宽度。

当然，如果把 viewport 的 width 属性设置为一个定值，比如 320、 480、 700 等等，那 viewport 的宽度即为设定的宽度。此时，设备独立像素宽度，也即所设定的宽度，而物理像素与设备独立像素的比值，则不再是最初始的设备像素比值了（比如 iPhone 4 中的2）。

现在的智能手机屏幕尺吋多样，分辨率有很多种，相应地，设备像素比也不一致，有0.75、1、1.5、2、2.25、等等，而在一般情况下（指 viewport 设置为 width=device-width 时）的设备独立像素宽度，也不再只是 320px 了，还有 360px 、 400px 、等等。也许，在你纠结完屏宽为什么是 320px 这个问题之后，就要开始纠结网页在不同手机屏幕上的显示问题了。


###一些概念










http://zh.wikipedia.org/wiki/%E8%A7%86%E7%BD%91%E8%86%9C%E6%98%BE%E7%A4%BA%E5%99%A8

http://en.wikipedia.org/wiki/Device_independent_pixel






device-width 就等于 320 么？屏宽（屏幕宽度）就等于 device-width 么？

对于设备屏幕的分辨率（也即物理像素数），我们能确切地从设备的参数列表中查到，比如 iPhone 4 的屏幕分辨率是 640px\*960px ，HTC G13 的屏幕分辨率是 320px\*480px 。屏幕分辨率和屏宽其实通常是一个倍数关系，比如，这个倍数在 js 中叫 devicePixelRatio ，称作“设备像素比”：

	设备像素比 = 屏幕分辨率 / 屏宽

上面公式里的屏宽，等于 viewport 设置为 width=device-width 时的宽度。

在查资料的时候，遇到了下面几个概念，不得不提：

	分辨率：
	DPI：英文 Dots per inch 的缩写，点每英寸（线长）
	PPI：英文 Pixel per inch 的缩写，像素每英寸（线长）
	dot：点。硬件设备最小显示单元
	pixel：像素。最小的图像单元，在屏幕上的表现为单个染色点。既可是一个点，又可是多个点的集合

点做为硬件设备的最小显示单元，应该是不可以再分的，而像素却可以是一个点，也可以是多个点的集合。就像一台14吋的桌面显示器最大分辨率是 1024px\*768px ，我们可以手动调整显示器的分辨率为 800px\*600px ，虽然显示分辨率变了，但显示器能显示的最小显示单元个数没变，仍然是 1024\*768 个，只不过由之前的一个像素对应一个显示单元变成了一个像素对应多个显示单元一样，那么手机显示屏的尺寸，是不是也可以理解为，**由于设备像素比重设了屏幕显示分辨率，让一个像素与显示单元的对应数发生改变了**呢？

	iPhone 4 的屏幕分辨率是 640px*960px ，设备像素比是2，因此屏宽为 320 。
	HTC G13 的屏幕分辨率是 320px*480px ，设备像素比是1，因此屏宽为 320 。
	Samsung Note 的屏幕分辨率是 800px*1280px ，设备像素比是2，因此屏宽为 400 。
	Samsung Note 2 的屏幕分辨率是 720px*1280px ，设备像素比是2，因此屏宽为 360 。

320px 的由来，大概是找到原因了。正是由于设备像素比这个“对应数”，让不同分辨率的手机有大致相同的显示分辨率，也就是屏宽。

关于这个问题，也许是涉及到了“原理”的部分，描述起来着实有些吃力，有点“只能意会，不可言传”的感觉，建议大家再看看 [张鑫旭的文章](http://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/) 和 [大漠的文章](http://www.w3cplus.com/css/towards-retina-web.html) ，综合理解一下。





###最后

由于手机上页面的显示原理，引起了比如页面尺寸兼容、图片清晰度等问题，虽然有大体上的认识，但跟别人解释的时候总是不能顺畅地说出来。这篇文章酝酿了大半年，一直苦于怎么才能把问题说清楚，现在终于有成。其中大多为自己的理解，如有不对的地方，还请指正，非常感谢！

Resolution vs. Density vs. Pixel Ratios
http://mir.aculo.us/2012/09/27/resolution-vs-density-vs-pixel-ratios/

优化你的网页在Retina屏幕上的显示效果
http://blog.sina.com.cn/s/blog_62ae03a4010116e1.html

