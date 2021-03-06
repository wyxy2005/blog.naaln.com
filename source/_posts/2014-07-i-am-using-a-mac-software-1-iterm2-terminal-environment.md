---
layout: post
title: 我在用的mac软件--终端环境之iTerm2
date: 2014/07/27 04:55:24
categories: 
- 技术
tags: 
---

之前一直有朋友要我分享下在用的mac软件，今天有空就来写一下，可能不止于软件，会有一些配置或者工具，或者叫环境更合适。有些可能一句话带过，有些会详细介绍。也不分类了，想到哪个就写出来。如果已经写的足够长了，就会分几篇来写。

### iTerm2+zsh+tmux的终端环境

![](https://ww1.sinaimg.cn/large/006tNc79gw1fahq3nmnljj30vq0irtbf.jpg)

# iTerm2

其实现在os x的terminal已经做的很好了，但是iTerm2的功能依然要更强大一些。简述下优点和常用功能：

* 开源免费。
* 兼容性比默认Terminal更好。 
* 对于经常要远程使用的情况下，默认的Terminal在使用vi时经常出现不兼容的问题，而iTerm2在这方面显然做的更好。
* 快捷键丰富。 
* `⌘ + 数字`: 切换标签页。 `⌘ + 方向键` 按方向切换标签页。
* `⌘ + enter`: 切换全屏
* `⌘ + f`: 查找。支持正则。其中查找的内容会被自动复制。省去了再去⌘+c的步骤。同样，鼠标去选中的内容也会自动复制，也可以鼠标中键直接粘贴。一般在使用时，键入搜索关键词，然后用`shift-tab`或者`tab`左右自动补全，`option + enter`则自动将搜索结果键入，并且复制到剪贴板。
* `⌘ + d`: 垂直分屏，`⌘ + shift + d`: 水平分屏。使用`⌘ + ]`和`⌘ + [`在最近使用的分屏直接切换.而`⌘ + opt + 方向键`切换到指定位置的分屏。
* `⌘ + t` :新的标签页
* `⌘ + w` :关闭当前标签页
* `⌘ + ；`:自动补全历史命令。如图: 

![][1]

* `⌘ + shift + h`: 剪贴板历史，如图： 

![][2]

* `ctrl + u`: 清空当前行。这里要注意，mac默认的ctrl+u为清楚当前光标至行首的内容，在iTerm2中则直接清除本行。iTerm2 也支持其他常用的操作命令，这里顺带讲下，因为这些命令都是mac下非常常用也很好用的： 
* `ctrl + a`: 到行首
* `ctrl + e`: 行末
* `ctrl + f/b`: 前进后退，相当于左右方向键，但是显然比移开手按方向键更快
* `ctrl + p`: 上一条命令，相当于方向键上
* `ctrl + r`: 搜索命令历史，这个大家都应该很熟悉了
* `ctrl + d`: 删除当前字符
* `ctrl + h`: 删除之前的字符
* `ctrl + w`: 删除光标前的单词
* `ctrl + k`: 删除到文本末尾
* `ctrl + t`: 交换光标处文本
* `⌘ + —/+/0`: 调整字体大小
* `⌘ + r`:清屏，其实是滚到新的一屏，并没有清空。ctrl + l 也可以做到。
* 更多实用功能。 
* Exposé 标签 按`⌘ + opt + e `打开Exposé，并支持搜索。如图： 

![][3]

* 全局呼出快捷键。如图： 

![][4]

* `⌘ + /`: 找到当前光标位置，有时会很有用。
* `shift + ⌘ + s`: 保存当前窗口快照。
* `⌘ + opt + b`: 快照回放。很有意思的功能，你可以对你的操作根据时间轴进行回放。可以拖动下方的时间轴，也可以按左右方向键。如图：

![][5]

* 支持256色。方便配置vi配色。但是在某些远超服务器上不支持256色，则只要在`Prefences->Profiles->Terminal`里设置为xterm。

[1]: https://ww3.sinaimg.cn/large/006tNc79gw1f510mjg2u0j317008c78m

[2]: https://ww4.sinaimg.cn/large/006tNc79gw1f510ms8jkqj315a0bg44h

[3]: https://ww3.sinaimg.cn/large/006tNc79gw1f510nmup8dj31kw0ur137

[4]: https://ww2.sinaimg.cn/large/006tNc79gw1f510nuwj21j30iw04ujrq

[5]: https://ww2.sinaimg.cn/large/006tNc79gw1f510o38ewlj311e08kabq
