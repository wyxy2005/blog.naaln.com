---
layout: post
title: 移动前端不得不了解的html5 head 头标签
date: 2015/09/24 13:21:00
categories: 
- 技术
tags: 
- html
---

本文主要内容来自一丝的常用的 [HTML 头部标签](https://github.com/yisibl/blog/issues/1)和百度FEX的[HTML head 头标签](http://fex.baidu.com/blog/2014/10/html-head-tags/)。

移动端的工作已经越来越成为前端工作的重要内容，除了平常的项目开发，HTML 头部标签功能，特别是meta标签显得非常重要。

### DOCTYPE

DOCTYPE(Document Type)，该声明位于文档中最前面的位置，处于 `html` 标签之前，此标签告知浏览器文档使用哪种 HTML 或者 XHTML 规范。

使用 HTML5 doctype，不区分大小写。

	<!DOCTYPE html> <!-- 使用 HTML5 doctype，不区分大小写 -->

	

### charset

声明文档使用的字符编码，

	<meta charset="utf-8">

html5 之前网页中会这样写：

	<meta http-equiv="Content-Type" content="text/html; charset=utf-8">

这两个是等效的，具体可移步阅读：[`<meta charset='utf-8'>` vs `<meta http-equiv='Content-Type'>`](http://stackoverflow.com/questions/4696499/meta-charset-utf-8-vs-meta-http-equiv-content-type)，所以建议使用较短的，易于记忆。

### lang属性

更加标准的 lang 属性写法 [http://zhi.hu/XyIa](http://zhi.hu/XyIa)

简体中文

	<html lang="zh-cmn-Hans"> <!-- 更加标准的 lang 属性写法 http://zhi.hu/XyIa -->

繁体中文

	<html lang="zh-cmn-Hant"> <!-- 更加标准的 lang 属性写法 http://zhi.hu/XyIa -->

很少情况才需要加地区代码，通常是为了强调不同地区汉语使用差异，例如：

	<p lang="zh-cmn-Hans">

	<p lang="zh-cmn-Hans">

	<strong lang="zh-cmn-Hans-CN">菠萝</strong>和<strong lang="zh-cmn-Hant-TW">鳳梨</strong>其实是同一种水果。只是大陆和台湾称谓不同，且新加坡、马来西亚一带的称谓也是不同的，称之为<strong lang="zh-cmn-Hans-SG">黄梨</strong>。

	</p>

为什么 `lang="zh-cmn-Hans"` 而不是我们通常写的 `lang="zh-CN"` 呢，请移步阅读: [页头部的声明应该是用 lang=”zh” 还是 lang=”zh-cn”](http://zhi.hu/XyIa)。

### 优先使用 IE 最新版本和 Chrome

	<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />

### 360 使用Google Chrome Frame

	<meta name="renderer" content="webkit">

360 浏览器就会在读取到这个标签后，立即切换对应的极速核。 另外为了保险起见再加入

	

	<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">

这样写可以达到的效果是如果安装了 Google Chrome Frame，则使用 GCF 来渲染页面，如果没有安装 GCF，则使用最高版本的 IE 内核进行渲染。

相关链接：[浏览器内核控制 Meta 标签说明文档](http://se.360.cn/v6/help/meta.html)

### 百度禁止转码

通过百度手机打开网页时，百度可能会对你的网页进行转码，脱下你的衣服，往你的身上贴狗皮膏药的广告，为此可在 head 内添加

	<meta http-equiv="Cache-Control" content="no-siteapp" />

相关链接：[SiteApp 转码声明](http://m.baidu.com/pub/help.php?pn=22&ssid=0&from=844b&bd_page_type=1)

### SEO 优化部分

*   页面标题`<title>`标签(head 头部必须)

	<title>your title</title>

	

*   页面关键词 keywords

	<meta name="keywords" content="your keywords">

*   页面描述内容 description

	<meta name="description" content="your description">

	

*   定义网页作者 author

	<meta name="author" content="author,email address">

*   定义网页搜索引擎索引方式，robotterms 是一组使用英文逗号「,」分割的值，通常有如下几种取值：none，noindex，nofollow，all，index和follow。

	<meta name="robots" content="index,follow">

相关链接：[WEB1038 – 标记包含无效的值](http://msdn.microsoft.com/zh-cn/library/ff724037(v=expression.40).aspx)

### 为移动设备添加 viewport

`viewport` 可以让布局在移动浏览器上显示的更好。 通常会写

	<meta name ="viewport" content ="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no"> <!-- `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边 http://bigc.at/ios-webapp-viewport-meta.orz -->

`width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边([http://bigc.at/ios-webapp-viewport-meta.orz](http://bigc.at/ios-webapp-viewport-meta.orz))

content 参数：

*   width viewport 宽度(数值/device-width)

*   height viewport 高度(数值/device-height)

*   initial-scale 初始缩放比例

*   maximum-scale 最大缩放比例

*   minimum-scale 最小缩放比例

*   user-scalable 是否允许用户缩放(yes/no)

*   minimal-ui iOS 7.1 beta 2 中新增属性（注意：iOS8 中已经删除），可以在页面加载时最小化上下状态栏。这是一个布尔值，可以直接这样写：

	<meta name="viewport" content="width=device-width, initial-scale=1, minimal-ui">

	

而如果你的网站不是响应式的，请不要使用 initial-scale 或者禁用缩放。

	<meta name="viewport" content="width=device-width,user-scalable=yes">

相关链接：[非响应式设计的viewport](http://www.qianduan.net/non-responsive-design-viewport.html)

适配 iPhone 6 和 iPhone 6plus 则需要写：

	<meta name="viewport" content="width=375">

	<meta name="viewport" content="width=414">

大部分 4.7~5 寸的安卓设备的 viewport 宽设为 360px，iPhone 6 上却是 375px，大部分 5.5 寸安卓机器（比如说三星 Note）的 viewport 宽为 400，iPhone 6 plus 上是 414px。

### ios 设备

添加到主屏后的标题（iOS 6 新增）

	<meta name="apple-mobile-web-app-title" content="标题"> <!-- 添加到主屏后的标题（iOS 6 新增） -->

	

是否启用 WebApp 全屏模式

	<meta name="apple-mobile-web-app-capable" content="yes" /> <!-- 是否启用 WebApp 全屏模式 -->

设置状态栏的背景颜色

	<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" /> <!-- 设置状态栏的背景颜色，只有在 `"apple-mobile-web-app-capable" content="yes"` 时生效 -->

只有在 “apple-mobile-web-app-capable” content=”yes” 时生效

content 参数：

*   default 默认值。

*   black 状态栏背景是黑色。

*   black-translucent 状态栏背景是黑色半透明。 如果设置为 default 或 black ,网页内容从状态栏底部开始。 如果设置为 black-translucent ,网页内容充满整个屏幕，顶部会被状态栏遮挡。

禁止数字识自动别为电话号码

	<meta name="format-detection" content="telephone=no" /> <!-- 禁止数字识自动别为电话号码 -->

### iOS 图标

rel 参数： apple-touch-icon 图片自动处理成圆角和高光等效果。 apple-touch-icon-precomposed 禁止系统自动添加效果，直接显示设计原图。 iPhone 和 iTouch，默认 57×57 像素，必须有

	<link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png" /> <!-- iPhone 和 iTouch，默认 57x57 像素，必须有 -->

iPad，72×72 像素，可以没有，但推荐有

	<link rel="apple-touch-icon-precomposed" sizes="72x72" href="/apple-touch-icon-72x72-precomposed.png" /> <!-- iPad，72x72 像素，可以没有，但推荐有 -->

Retina iPhone 和 Retina iTouch，114×114 像素，可以没有，但推荐有

	<link rel="apple-touch-icon-precomposed" sizes="114x114" href="/apple-touch-icon-114x114-precomposed.png" /> <!-- Retina iPhone 和 Retina iTouch，114x114 像素，可以没有，但推荐有 -->

Retina iPad，144×144 像素，可以没有，但推荐有

	<link rel="apple-touch-icon-precomposed" sizes="114x114" href="/apple-touch-icon-114x114-precomposed.png" /> <!-- Retina iPhone 和 Retina iTouch，114x114 像素，可以没有，但推荐有 -->

IOS 图标大小在iPhone 6 plus上是180×180，iPhone 6 是120×120。 适配iPhone 6 plus，则需要在中加上这段

	<link rel="apple-touch-icon-precomposed" sizes="180x180" href="retinahd_icon.png">

### iOS 启动画面

官方文档：[https://developer.apple.com/library/ios/qa/qa1686/_index.html](https://developer.apple.com/library/ios/qa/qa1686/_index.html) 参考文章：[http://wxd.ctrip.com/blog/2013/09/ios7-hig-24/](http://wxd.ctrip.com/blog/2013/09/ios7-hig-24/)

iPad 的启动画面是不包括状态栏区域的。

iPad 竖屏 768 x 1004（标准分辨率）

	<link rel="apple-touch-startup-image" sizes="768x1004" href="/splash-screen-768x1004.png" /> <!-- iPad 竖屏 768 x 1004（标准分辨率） -->

iPad 竖屏 1536×2008（Retina）

	<link rel="apple-touch-startup-image" sizes="1536x2008" href="/splash-screen-1536x2008.png" /> <!-- iPad 竖屏 1536x2008（Retina） -->

iPad 横屏 1024×748（标准分辨率）

	<link rel="apple-touch-startup-image" sizes="1024x748" href="/Default-Portrait-1024x748.png" /> <!-- iPad 横屏 1024x748（标准分辨率） -->

iPad 横屏 2048×1496（Retina）

	<link rel="apple-touch-startup-image" sizes="2048x1496" href="/splash-screen-2048x1496.png" /> <!-- iPad 横屏 2048x1496（Retina） -->

iPhone 和 iPod touch 的启动画面是包含状态栏区域的。

iPhone/iPod Touch 竖屏 320×480 (标准分辨率)

	<link rel="apple-touch-startup-image" href="/splash-screen-320x480.png" /> <!-- iPhone/iPod Touch 竖屏 320x480 (标准分辨率) -->

iPhone/iPod Touch 竖屏 640×960 (Retina)

	<link rel="apple-touch-startup-image" sizes="640x960" href="/splash-screen-640x960.png" /> <!-- iPhone/iPod Touch 竖屏 640x960 (Retina) -->

iPhone 5/iPod Touch 5 竖屏 640×1136 (Retina)

	<link rel="apple-touch-startup-image" sizes="640x1136" href="/splash-screen-640x1136.png" /> <!-- iPhone 5/iPod Touch 5 竖屏 640x1136 (Retina) -->

添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）

	<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL"> <!-- 添加智能 App 广告条 Smart App Banner（iOS 6+ Safari） -->

iPhone 6对应的图片大小是750×1294，iPhone 6 Plus 对应的是1242×2148 。

	<link rel="apple-touch-startup-image" href="launch6.png" media="(device-width: 375px)">

<link rel="apple-touch-startup-image" href="launch6plus.png" media="(device-width: 414px)">

### Android

Android Lollipop 中的 Chrome 39 增加 theme-color meta 标签，用来控制选项卡颜色。

[http://updates.html5rocks.com/2014/11/Support-for-theme-color-in-Chrome-39-for-Android](http://updates.html5rocks.com/2014/11/Support-for-theme-color-in-Chrome-39-for-Android)

![687474703a2f2f757064617465732e68746d6c35726f636b732e636f6d2f6173736574732f7468656d652d636f6c6f722d73732d39393530666431336130633532623332623662643538303330396435356136622e706e67](https://ww3.sinaimg.cn/large/006tNc79gw1fahq68wn6pj309e0godho.jpg)

	<meta name="theme-color" content="#db5945">

### Windows 8

Windows 8 磁贴颜色

	<meta name="msapplication-TileImage" content="icon.png"/> <!-- Windows 8 磁贴图标 -->

Windows 8 磁贴图标

<pre class="prettyprint lang-html linenums:1 prettyprinted">

1.  <span class="tag"><meta</span> <span class="pln"></span> <span class="atn">name</span><span class="pun">=</span><span class="atv">"msapplication-TileImage"</span> <span class="pln"></span> <span class="atn">content</span><span class="pun">=</span><span class="atv">"icon.png"</span><span class="tag">/></span> <span class="pln"></span> <span class="com"><!-- Windows 8 磁贴图标 --></span>

</pre>

### rss订阅

	<link rel="alternate" type="application/rss+xml" title="RSS" href="/rss.xml" /> <!-- 添加 RSS 订阅 -->

### favicon icon

	<link rel="shortcut icon" type="image/ico" href="/favicon.ico" /> <!-- 添加 favicon icon -->

比较详细的 favicon 介绍可参考[https://github.com/audreyr/favicon-cheat-sheet](https://github.com/audreyr/favicon-cheat-sheet)

### 关闭chrome浏览器下翻译插件

有些时候感觉chrome浏览器下翻译插件很烦人，可以通过下面的代码禁用它。

	<meta name="google" value="notranslate" />

![A70DF5F6-374B-448D-B433-EB99081A9F13](https://ww2.sinaimg.cn/large/006tNc79gw1fahq6a0ecaj30us03gwfc.jpg)

### 移动端的头部标签和meta

```
	<!DOCTYPE html> <!-- 使用 HTML5 doctype，不区分大小写 -->

	<html lang="zh-cmn-Hans"> <!-- 更加标准的 lang 属性写法 http://zhi.hu/XyIa -->

	<head>
   <!-- 声明文档使用的字符编码 -->
   <meta charset='utf-8'>
   <!-- 优先使用 IE 最新版本和 Chrome -->
   <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
   <!-- 页面描述 -->
   <meta name="description" content="不超过150个字符"/>
   <!-- 页面关键词 -->
   <meta name="keywords" content=""/>
   <!-- 网页作者 -->
   <meta name="author" content="name, email@gmail.com"/>
   <!-- 搜索引擎抓取 -->
   <meta name="robots" content="index,follow"/>
   <!-- 为移动设备添加 viewport -->
   <meta name="viewport" content="initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no">
   <!-- `width=device-width` 会导致 iPhone 5 添加到主屏后以 WebApp 全屏模式打开页面时出现黑边 http://bigc.at/ios-webapp-viewport-meta.orz -->
   <!-- iOS 设备 begin -->
   <meta name="apple-mobile-web-app-title" content="标题">
   <!-- 添加到主屏后的标题（iOS 6 新增） -->
   <meta name="apple-mobile-web-app-capable" content="yes"/>
   <!-- 是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏 -->
   <meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL">
   <!-- 添加智能 App 广告条 Smart App Banner（iOS 6+ Safari） -->
   <meta name="apple-mobile-web-app-status-bar-style" content="black"/>
   <!-- 设置苹果工具栏颜色 -->
   <meta name="format-detection" content="telphone=no, email=no"/>
   <!-- 忽略页面中的数字识别为电话，忽略email识别 -->
   <!-- 启用360浏览器的极速模式(webkit) -->
   <meta name="renderer" content="webkit">
   <!-- 避免IE使用兼容模式 -->
   <meta http-equiv="X-UA-Compatible" content="IE=edge">
   <!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
   <meta name="HandheldFriendly" content="true">
   <!-- 微软的老式浏览器 -->
   <meta name="MobileOptimized" content="320">
   <!-- uc强制竖屏 -->
   <meta name="screen-orientation" content="portrait">
   <!-- QQ强制竖屏 -->
   <meta name="x5-orientation" content="portrait">
   <!-- UC强制全屏 -->
   <meta name="full-screen" content="yes">
   <!-- QQ强制全屏 -->
   <meta name="x5-fullscreen" content="true">
   <!-- UC应用模式 -->
   <meta name="browsermode" content="application">
   <!-- QQ应用模式 -->
   <meta name="x5-page-mode" content="app">
   <!-- windows phone 点击无高光 -->
   <meta name="msapplication-tap-highlight" content="no">
   <!-- iOS 图标 begin -->
   <link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png"/>
   <!-- iPhone 和 iTouch，默认 57x57 像素，必须有 -->
   <link rel="apple-touch-icon-precomposed" sizes="114x114" href="/apple-touch-icon-114x114-precomposed.png"/>
   <!-- Retina iPhone 和 Retina iTouch，114x114 像素，可以没有，但推荐有 -->
   <link rel="apple-touch-icon-precomposed" sizes="144x144" href="/apple-touch-icon-144x144-precomposed.png"/>
   <!-- Retina iPad，144x144 像素，可以没有，但推荐有 -->
   <!-- iOS 图标 end -->
   <!-- iOS 启动画面 begin -->
   <link rel="apple-touch-startup-image" sizes="768x1004" href="/splash-screen-768x1004.png"/>
   <!-- iPad 竖屏 768 x 1004（标准分辨率） -->
   <link rel="apple-touch-startup-image" sizes="1536x2008" href="/splash-screen-1536x2008.png"/>
   <!-- iPad 竖屏 1536x2008（Retina） -->
   <link rel="apple-touch-startup-image" sizes="1024x748" href="/Default-Portrait-1024x748.png"/>
   <!-- iPad 横屏 1024x748（标准分辨率） -->
   <link rel="apple-touch-startup-image" sizes="2048x1496" href="/splash-screen-2048x1496.png"/>
   <!-- iPad 横屏 2048x1496（Retina） -->
   <link rel="apple-touch-startup-image" href="/splash-screen-320x480.png"/>
   <!-- iPhone/iPod Touch 竖屏 320x480 (标准分辨率) -->
   <link rel="apple-touch-startup-image" sizes="640x960" href="/splash-screen-640x960.png"/>
   <!-- iPhone/iPod Touch 竖屏 640x960 (Retina) -->
   <link rel="apple-touch-startup-image" sizes="640x1136" href="/splash-screen-640x1136.png"/>
   <!-- iPhone 5/iPod Touch 5 竖屏 640x1136 (Retina) -->
   <!-- iOS 启动画面 end -->
   <!-- iOS 设备 end -->
   <meta name="msapplication-TileColor" content="#000"/>
   <!-- Windows 8 磁贴颜色 -->
   <meta name="msapplication-TileImage" content="icon.png"/>
   <!-- Windows 8 磁贴图标 -->
   <link rel="alternate" type="application/rss+xml" title="RSS" href="/rss.xml"/>
   <!-- 添加 RSS 订阅 -->
   <link rel="shortcut icon" type="image/ico" href="/favicon.ico"/>
   <!-- 添加 favicon icon -->
   <title>标题</title>

	</head>
```
	

更多的 meta 标签参考

*   [COMPLETE LIST OF HTML META TAGS](http://code.lancepollard.com/complete-list-of-html-meta-tags/)

*   [18 Meta Tags Every Webpage Should Have in 2013](http://www.iacquire.com/blog/18-meta-tags-every-webpage-should-have-in-2013)

参考文章：

*   [常用的 HTML 头部标签](https://github.com/yisibl/blog/issues/1)

*   [html5_header](https://gist.github.com/paddingme/6182708733917ae36331)

*   [amazeui css](http://amazeui.org/css/)

*   [DOCTYPE](http://www.douban.com/note/170560091/)

*   [WEB 工程师和设计师必学的 10 个 IOS 8 新鲜改变](http://www.uisdc.com/ios8-ten-new-feature)

</div>
