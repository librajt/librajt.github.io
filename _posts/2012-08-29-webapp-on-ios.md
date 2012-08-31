---
layout: post
title: "让WebApp在iOS上模拟NativeApp的效果"
description: "通过meta标签的设置，为添加到主屏的WebApp增加NativeApp的效果。"
category: 
tags: [webapp, iOS, media query]
---
{% include JB/setup %}

当下， WebApp 与 NativeApp 打的火热， WebApp 借助各种低成本的优势在 HTML5 大潮中与 NativeApp 叫板。虽然它的性能短板目前还不能完全动摇 NativeApp 的地位，但它的 NativeApp-Like 表现，足够让 HTML5er 兴奋一阵了。

WebApp 说穿了，就是一个符合 HTML5 标准的 web 站点，只不过会对主要应用设备——移动设备做了优化。在 HTML5 的帮助下，把一个 WebApp 站点包装成为 NativeApp 的形式，只需要添加几行简单的代码而已。

###基本设置

首先，为了保证 WebApp 页面能够在移动设备上完美的显示，需要设置 viewport 标签，使页面适应设备的屏幕宽度

	<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0">

关于 WebApp 页面如何在移动设备上显示，后面将会有篇 [文章](#todo) 详细说明。

保证 WebApp 页面的正常显示后，设置页面可以全屏运行

	<meta name="apple-mobile-web-app-capable" content="yes">

或者称为独立模式 (standalone mode) 运行。可以通过 js 方法 window.navigator.standalone 属性来区别是 WebApp 是独立模式运行还是浏览器模式运行。

到此为止，你的 WebApp 已经可以实现 NativeApp 的表现了。在浏览器中将 WebApp 添加到主屏，通过主屏的快捷方式访问，发现 WebApp 已经可以全屏运行了。

除此之外，还可以为 WebApp 添加更多设置让 WebApp 更加 NativeApp-Like 。

###自定义 icon

你会发现，添加 WebApp 到主屏时，系统会将当前浏览的页面进行截图以生成一个 icon ，这个 icon 是通过设置 apple-touch-icon 自定义

	<link rel="apple-touch-icon-precomposed" 
		href="ico-ipad.png">
	<link rel="apple-touch-icon"             
		href="ico-ipad-retina.png">
	
	<link rel="apple-touch-icon"             
		href="ico-iphone.png">
	<link rel="apple-touch-icon-precomposed" 
		href="ico-iphone-retina.png">

上面的四行代码，通过对比 href 属性指向的文件名，可以判断它们的用处吧。这里区别了 iPhone 和 iPad ，是由于两类设备使用的 icon 尺寸是不一致的，而且，对于是否 Retina 屏，为了达到较好的显示效果，也建议使用不同的尺寸。它们使用的尺寸如下：

	尺寸	普通屏	Retina
	iPad	72x72	144x144
	iPhone	57x57	114x114

图片尺寸必须严格要求，否则不能正常使用。图片格式建议 png24 。

此外，如果 apple-touch-icon 后面增加一个 -precomposed ，那么显示在主屏上的 icon 会被系统增加一个高光效果。

###自定义启动画面

优质的 NativeApp 会在程序启动时显示启动画面，在提升产品气质的同时，也能顺便做点适当的产品宣传。为 WebApp 添加启动画面，是通过设置 apple-touch-startup-image 

	/* for iPad */
	<link rel="apple-touch-startup-image"  
		href="startup-ipad-landscape.png"        
		media="(device-width:768px) and (orientation:landscape)">
	<link rel="apple-touch-startup-image"  
		href="startup-ipad-portrait.png"         
		media="(device-width:768px) and (orientation:portrait) ">
	<link rel="apple-touch-startup-image"  
		href="startup-ipad-retina-landscape.png" 
		media="(device-width:768px) and (orientation:landscape) 
			and (-webkit-min-device-pixel-ratio: 2)">
	<link rel="apple-touch-startup-image"  
		href="startup-ipad-retina-portrait.png"  
		media="(device-width:768px) and (orientation:portrait)  
			and (-webkit-min-device-pixel-ratio: 2)">

这是为 WebApp 在 iPad 上添加启动动画，除了像 icon 一样区分了是否 Retina 屏外，还区分了屏幕方向 orientation 。每种情况使用的启动图片尺寸如下：

	尺寸	普通屏		Retina
	横屏	1024x748	2048x1496
	竖屏	768x1004	1536x2008

图片格式可以为 png24 或 jpg 。相对于添加自定义 icon 的设置代码，这里的 link 标签还增加了一个 media 属性，它使用了 media query 来标识 iPad 设备并区分横竖屏，否则，相同的代码会前后覆盖，影响效果。关于 media query 后面将会有篇 [文章](#todo) 详细说明。

这里要说明一下， iPad 上支持的横竖屏启动画面，无关乎是在哪种情况下添加书签到主屏的。但是 iPhone 是不支持横屏启动的，有兴趣的话，可以用下面的代码试试

	/* for iPhone 过度的写法 */
	<link rel="apple-touch-startup-image" 
		href="startup-iphone-landscape.png"        
		media="(device-width:320px) and (orientation:landscape)">
	<link rel="apple-touch-startup-image" 
		href="startup-iphone-portrait.png"         
		media="(device-width:320px) and (orientation:portrait) ">
	<link rel="apple-touch-startup-image" 
		href="startup-iphone-retina-landscape.png" 
		media="(device-width:320px) and (orientation:landscape) 
			and (-webkit-min-device-pixel-ratio: 2)">
	<link rel="apple-touch-startup-image" 
		href="startup-iphone-retina-portrait.png"  
		media="(device-width:320px) and (orientation:portrait)  
			and (-webkit-min-device-pixel-ratio: 2)">

起初以为是我设置或打开的方式不对，后来一想，在 iPhone 上，主屏幕确实不可以横过来呀。好吧，可以做为依据了。于是，比较优质的写法是

	/* for iPhone 优质的写法 */
	<link rel="apple-touch-startup-image" 
		href="startup-iphone-portrait.png"        
		media="(device-width:320px)">
	<link rel="apple-touch-startup-image" 
		href="startup-iphone-retina-portrait.png" 
		media="(device-width:320px) and (-webkit-min-device-pixel-ratio: 2)">

两种情况下图片的尺寸如下：

	尺寸	普通屏		Retina
	iPhone	320x460		640x920

对于设置 icon 与启动画面，可以用不同设备浏览 [这个页面](/WebAppHelper/package/AddToHomeScreen) 并添加到主屏查看分别使用的图片。

以上内容在 iPhone4S/iOS5 、 iPod4/iOS5 、 iPad2/iOS5 上测试通过，欢迎在其它设备进行测试，感谢反馈。

###最后

如果你习惯手动输入代码，完成设置后仍然不能达到效果，请一定检查拼写，不要与我犯同样的低级错误...两次 Orz...

另外，你也许注意到了，是否 Retina 屏幕会让使用的图片尺寸两倍数关系。当然，也可以不区分是否 Retina 屏而中使用小尺寸的图片，但这样在 Retina 屏上查看时，图片会变得模糊。嗯，这是一个图片和页面在移动设备上的显示问题，期待后文:D













 


