---
title: 《OS X 进阶使用》－ 第二篇 自动化与 Alfred
date: 2016-01-05
---

## 自动化
OS X 自动化是一个很大的话题，可以分很多小的点来展开，这一篇讲一下 Alfred 这款软件。

## Alfred
Alfred 是 OS X 上的神器，跟 OS X 自带的 Spotlight 功能相近，同属于搜索入口，几乎就是本地的 Google。相较于 Spotlight，Alfred 支持更多的功能和更加强大的扩展。

Alfred 的入口很唯一，按下一个快捷键，弹出一个搜索框。在搜索框里，你可以查找文件，打开应用，计算器，管理剪切板，搜索网页，搜索 Wikipedia，执行 Shell 脚本，这些都是基本功能，大概已经能满足日常使用中 80％ 的需求。

### Alfred Workflows
Alfred 更强大的地方在于，支持 workflows 扩展。workflows 作用是通过搜索前缀跟脚本，扩展搜索功能。形象一点而言，搜索框就是一个入口，好比百度之前推的框计算。搜索框不仅仅是搜索功能的入口，它成了任何你想要的功能的入口。

这里给出一个链接，[Packal](http://www.packal.org/)，列出了几乎所有的社区贡献的 workflows。（有句话说的好，只有天空才是你的极限。）

如果上述列出的 workflows 还不能满足你特定的需求，那你也可以自己动手写一个扩展，它支持 Bash、Python、PHP、Ruby、AppleScript、JavaScript 等脚本语言，只要去响应特定的检索关键词，然后执行你自己的任务（例如，监控线上服务等等），最后给出一个输出，很像常见的 Shell 脚本干的事，只不过它的执行入口改成了统一的 Alfred 入口。

这文章写的有点安利了。。。Alfred 本身是免费的，80% 的功能免费试用，支持 workflows 的 Powerpack 售价 17 欧元。在 OS X 和 iOS 的环境里，好东西基本都不是免费的，希望大家能体会到。不理解软件为什么要收费的同学，想想你每天干的事，再想想老板为啥要付你工资。

Windows 用户可以看一下 [Launchy](http://www.launchy.net/) 这款免费软件，类似的功能定位，只不过功能上没有 Alfred 强大。

最后提一句，我把 Aflred 快捷键改成了 CapsLock 键，它是最容易按到的一个键，而平时又没有多大用处，用来随手启动 Alfred 刚刚好。至于为何这么改，以及如何改，正好关联到下一个话题，《键盘与 Keyboard Maestro》。