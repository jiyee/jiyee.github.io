---
title: Appecelerator Titanium开发经验谈之一 - PhoneGap与Titanium的艰难选择 #一个前端工程师的App之路
date: 2014-08-05
---

跟 PhoneGap、AppCan 类似，Appecelerator Titanium（简称**Ti**）致力于使用 JavaScript 去快速构建 App ([Atwood定律]在发功)。它们有效地抚平了学习曲线，使得 App 开发不再依赖于对 Objective-C 或 Java 语言以及相应开发框架，转而建立在 JavaScript、V8 和 Node 基础之上，让我等前端工程师在 App 开发大潮中有机会去施展。

好像在中文社区谈论 PhoneGap 的相对多一些，至少搜索相关的 Titanium 资料就比较少。但是，在 V2EX 上关于 PhoneGap 的讨论都基本停留在它性能如何不好怎么后悔方面，但是一看发表时间好像也是 1，2 年以前的事了，这让我对此更加疑惑。按常理说，一个前端工程师肯定更加倾向于 PhoneGap ，就是一个 WebView 么，以前玩的那些都能在 App 上跑起来，多好啊。不过在深入对比过 PhoneGap 和 Titanium 的区别之后，还是抛弃了 PhoneGap ，即便 PhoneGap+Ionic+AngularJS 能够快速开发出一个复杂的、不错的应用，而 Titanium 能够开发出一个更加接近原生（Native）的 App 。

PhoneGap 和 Ti 之间的艰难选择的结论主要集中到以下三点：

-  一是产品性能问题，虽然我没有拿 PhoneGap 来开发出一个较复杂 App ，但是之前浏览器环境下的开发经验告诉我，在一个 WebView 上开发 App ，基本上也不会避开浏览器上的那些问题，包括内容加载、状态维护、页面切换、动态效果等。

- 二是框架定位问题，PhoneGap 主要还是引领 W3C 标准，致力于前瞻性的开发一些 Device API ，例如 Audio、Camera、Geolocation、localStorage 等，它提供是对于这些 Device API 的 bridge 。但是，它对于 App 本身关注较少，这也注定了 PhoneGap 的目标是让 webapp 获得更多的 Device API ，而不是构建原生的 App 。具体请参见[《跨平台移动开发工具:PhoneGap与Titanium全方位比拼》]一文，原文参见[《Comparing Titanium and PhoneGap》]（很棒的一篇文章）。

- 三是 UI 问题，PhoneGap 本身不提供任何 UI 相关的接口，一切 UI 都由 HTML 实现，当然也出现了很多专门的 UI 框架，比较优秀的有 Ionic，Lungo，所以他开发出来的 UI 一致性很好，真的成也萧何败萧何，好坏自知。

  Ti 并没有极力去说服开发者一套代码适用于全平台，它更加强调代码的重用度，例如 Model 代码重用度 90 %， View 层代码重用度 80 %。这与 Ti 框架的设计有关，它是一个底层 Native API 的 PORT ，当然也包括了 UI 相关的 API ，它更加接近于原生 App ，这里没有说他是原生 App 是因为它仍然是在运行时再去执行你的 JavaScript 代码，并没有直接将 JavaScript 编译到 Objective-C 或者 Java ，甚至是字节码。所以，相比原生 App 它在性能上也会打个折。

![Ti Flow](/images/ti-flow.png)

附图就是 Ti 框架下 JavaScript 最终如何与底层 API 交互的逻辑，Ti 就是作为 Proxy，在运行时动态解析 JavaScript 代码（在 V8/JavaScript Core 环境下），然后 Proxy 到 Ti SDK ，再 Proxy 到底层 API。

看来是很不错的构想，距离 JavaScript 一统天下不远了，好吧，就这么定了，撇下 PhoneGap ，搭上 Titanium。