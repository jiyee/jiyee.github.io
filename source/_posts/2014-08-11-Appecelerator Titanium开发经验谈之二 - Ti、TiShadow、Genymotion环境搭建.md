---
title: Appecelerator Titanium开发经验谈之二 - Ti、TiShadow、Genymotion环境搭建 #一个前端工程师的App之路
date: 2014-08-11
---

首先，不得不说最近一年，Ti 的文档得到了明显地提升，只要认真看一遍文档，基本的流程

Titanium 环境搭建还是比较简单的，基本都能在 http://docs.appcelerator.com/titanium/latest/#!/guide/Quick\_Start 找到。

* 安装 Java，Node 环境
* 安装 Android platform SDKs，以及对应版本的 Android SDK
* 安装 Titanium SDK，目前是 3.3.0
* 安装 Titanium CLI 和 Allow CLI
* 安装 Titanium Studio

这样就完成了基本的 Android Titanium 开发环境。

另外，Android 虚拟机的话，强烈推荐使用 [Genymotion](http://www.genymotion.com/)，比默认的快很多，而且很容易多版本部署和升级。

如果想快速开发的话，那最好装上 [TiShadow](http://tishadow.yydigital.com/)，类似的还有 [RapidDev](https://github.com/appersonlabs/RapidDev)，他是在 Ti SDK 之上，再封装了一层 runtime，将本地的 Titanium+Alloy 代码进行动态编译，并在 TiShadow 运行时上进行替换更新，实现 LiveView 效果。

不过在 Genymotion 上运行 TiShadow 还需要安装对应 Android 版本的 gapps。

其他好像就没有什么了。