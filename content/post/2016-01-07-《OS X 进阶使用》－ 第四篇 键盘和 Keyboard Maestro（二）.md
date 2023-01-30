---
title: 《OS X 进阶使用》－ 第四篇 键盘和 Keyboard Maestro（二）
date: 2016-01-07
---

## 前言
本来这个段落的标题是「价值观」，我想想有点过了，还是改成「前言」好了。

使用键盘方面，我的判断是：能够有效地使用键盘完成工作是衡量程序员开发效率标准之一。

我见过键盘输入速度和熟练度上较高的一名选手就是 [browserify](http://browserify.org) 的作者 [James Halliday](http://substack.net)，大家有兴趣可以上 [Youtube](https://m.youtube.com/results?q=james%20halliday%20substack&sm=1) 上看看他做的分享的一些视频。

无论在 OS X 还是 Windows 平台上，想要高效地使用键盘都需要花费一番功夫，并不是打字足够快就能够说明高效，而是看你如何使用键盘完成一件事的方式。

当然，首要的一点就是你能快速准确地使用键盘完成文本输入，尤其是代码输入，大家有兴趣可以上 [Typing Practice for Programmers](http://typing.io) 这个网站测试一下你输入代码的速度，我的 WPM 是 45+，无效按键是 19%。

## 快捷键与模式
除了快速、准确地文本输入之外，熟记足够多的快捷键，也是提高效率的重要一方面。

在 OS X 上所有的输入框和 Terminal 快捷键模式是 [Emacs 模式](http://jblevins.org/log/kbd)，如果你是 Emacs 党，那你一定如鱼得水，如果你是 Vim 党，那可能你也得学会如何使用 Emacs 的快捷键。无论是普通的文本输入框还是 Terminal 都是 OS X 用户每日都接触的，熟练使用快捷键就能减少鼠标的使用或者一路按左或者按右的状况。

作为一个伪 Vim 党，平日我基本不用 Vim 编辑器，只是保持在最常用场景中尽可能使用 [Vim 模式](https://en.m.wikibooks.org/wiki/Learning_the_vi_Editor/Vim/Modes)。在 Chrome 浏览器，我使用 [cvim](https://chrome.google.com/webstore/detail/cvim/ihlenndgcmojhcghmfjfneahoeklbjjh?hl=en) 插件，只需要键盘就能完成 URL 输入，页面滚动，链接跳转等工作，完全不用鼠标。在 Sublime Text，有 [Vintageous](https://github.com/guillermooo/Vintageous) 插件，在 Jetbrains 家的 AppCode，有 [IdeaVim](https://github.com/JetBrains/ideavim) 插件，这些插件除了一些宏的设置和使用上存在一定缺陷之外，基本都能保证 Vim 模式。在其他软件中，通过快捷键映射，把 Hyper＋H/J/K/L 映射到了左下上右等。

我既不是 Emacs 用户，也不是 Vim 用户，很难说使用他们究竟有什么优势，大家有兴趣可以看看 [「编辑器之战」](https://zh.wikipedia.org/zh/%E7%BC%96%E8%BE%91%E5%99%A8%E4%B9%8B%E6%88%98) 这个 Wiki。但是，作为一名伪 Vim 党，我想说的是，统一使用 Emacs 或者 Vim 模式，能尽量让你保持在移动、输入和修改等操作过程中，头脑的思考方式是一致的，而且尽量避免了鼠标操作。

键盘这部分已经写了两篇，看起来也很难把内容都涵盖到，我只能尽量把我认为一些重要的内容提出来简单讲一讲，希望对大家有所帮助和启发。
