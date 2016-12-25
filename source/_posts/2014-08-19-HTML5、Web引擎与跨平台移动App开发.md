---
title: '[转]HTML5、Web引擎与跨平台移动App开发'
date: 2014-08-19
---

移动端跨平台应用开发是个有趣的话题。纵观该领域目前各个开发商提供的多种方案，大致可以分为三大类：

1. 基于HTML5的方案。该方案以PhoneGap/Cordova为代表。其基本思路是针对HTML5标准目前功能上的不足，补充定义了一套比较实用的API（比如硬件访问/系统交互等），然后基于平台上自带的Web引擎（比如iOS的UIWebview等），通过扩展机制实现了这些API，在此基础上再提供一套应用打包部署系统。Intel的XDK也属于此类方案。

2. 将Native API映射封装成统一语言的API的方案。该方案以Titanium、Xamarin为代表，其中Titanium提供JavaScript API，Xamarin提供C# API。这样的好处是可以较容易达到和Native API类似的能力，编程模型/方式也和原生应用相似。

3. 有行业针对性的HTML5 API方案。比如Ludei的CocoonJS就是一个比较有意思的方案，它设计了一套专门针对2D/3D游戏开发的API（支持iOS和Android）。可以认为它是HTML5图形操作的子集（Canvas +WebGL），再加上一些扩展的API比如硬件访问能力/广告/应用内购买/社交网络整合等，以实现一个完整的游戏引擎。

本文重点介绍基于HTML5的方案相比其他方案的优缺点，如何实现更好的效果，以及目前的一些进展。

### HTML5方案的特点

原生API映射的方案，如Titanium、Xamarin，其优点在于功能和性能与原生系统比较接近。但是，由于不同系统原生API设计上还是会有不少差异，API的映射还是需要不少的权衡取舍。同时，由于这些API是这些厂商自定义的，谈不上什么标准，相应的开发资源（程序库/技术支持/社区等等）也相对有限。

而另一方面，标准化、开发资源的丰富则是HTML5方案最大的优点，同时第三方的HTML5框架工具比如PhoneGap/Cordova也极大促进了HTML5应用的发展，它们提供了方便的跨平台应用打包/发布服务、实用的API、灵活的扩展机制、以及积累下来的丰富的第三方API实现。而上游的W3C一旦开始支持一些新的API，PhoneGap/Cordova也可以很快沿用这些标准的API将相关能力开放出去。

HTML5方案的主要不足则在于功能和性能方面，这主要是因为HTML5应用的能力严重依赖于系统自带的Web引擎：iOS的UIWebview、Android的Webview等，此类组件的HTML5能力相比Safari for iOS、Chrome for Android都要差一截。另外在Android平台上，由于系统碎片化比较严重，不同Android版本的Webview的HTML5能力也有较大差异，导致相应的HTML5应用一致性难以保证。

好消息是，现在已经出现一些第三方的Web引擎以提供比系统默认的Webview更好的功能和性能，而PhoneGap/Cordova也正在改进架构以便引入这些更好的第三方Web引擎。另外对于Tizen、Firefox OS这样本身就是HTML5 Runtime加上扩展API的系统而言，HTML5应用是一等公民，在功能拓展方面相比iOS、Android上会增强不少。

而第三种方案，CocoonJS的优点是专注于2D/3D游戏开发，画图性能很好，比如同时画1000个精灵也能达到60FPS，这是绝大多数的浏览器/通用的HTML5引擎目前还做不到的。这个方案的缺点在于，由于它的画图操作简化了很多路经，它无法做到和HTML5 DOM元素的互操作，而且它的HTML5能力也只是一个子集，功能比较受限。目前CocoonJS针对Android也引入了另外一种模式Webview+作为补充，Webview+基于Chromium的内核加上Cordova API的支持以实现更通用的HTML5能力。

总的来说，HTML5应用的能力很大程度上依赖于Web引擎的能力。因此，无论是移动操作系统开发商还是开发工具的开发商，都持续在Web引擎的方向投入了更多的努力。

### Web引擎

Web引擎目前大致可分为三种方式：

1. 浏览器，比如Safari/Chrome/UC Browser等;
2. 系统自带的Webview组件，比如上面提到的iOS UIWebview和Android Webview
3. 专门的Web Engine，比如Intel的开源项目Crosswalk、Ludei的Webview+

浏览器方式很容易理解，一个HTML5应用就是一个Web页面，用户通过浏览器打开一个URL，然后进入浏览器的全屏模式/App模式进行操作，或者是通过点击一个事先创建好的快捷方式打开应用。这种方式的性能取决于浏览器本身对HTML5的支持情况，一般来说要优于Webview组件的方式，但是问题在于不同的浏览器有差异，而且通过浏览器运行HTML5较难做到类似原生应用的体验（应用切换/权限管理/系统资源访问/整合等）以及丰富的API支持。

Webview组件方式的一般用法是以Hybrid的方式发布HTML5应用，即上述提到的PhoneGap/Cordova方案所采用的方式。其问题已经在上面提到过，主要是Webview组件本身对HTML5的支持能力不足。

专门的Web引擎可以有较好的HTML5功能和性能支持，同时有较好一致性，类似原生应用的系统整合也可以做得较好。这种方式的缺点则在于开发者需要将Web引擎与应用程序一起打包，生成的应用大小会更大，因此有的Web引擎（如Crosswalk）也提供了一种“共享模式”，让多个应用可以共享一个Web引擎，仅当应用第一次启动并且发现系统还没有相应Web引擎时才提示用户下载安装。

目前的发展趋势是：通过PhoneGap/Cordova方式得到丰富的API支持，通过专门开发的Web引擎去提升HTML5的能力。

Crosswalk和Ludei的Webview+在概念上比较类似。Webview+是闭源的，目前还不好评估；Crosswalk由我所在的团队开发，是开源的（BSD许可协议），基于Chromium内核，着重于对HTML5功能和性能的支持，发布周期为六周一次，支持Cordova API。

目前Crosswalk正式支持的移动操作系统包括Android和Tizen，在Android 4.0及以上的系统中使用Crosswalk的Web应用程序体验和原生应用没有区别。该引擎现在已经成为众多知名HTML5平台和应用的推荐引擎，包括Google Mobile Chrome App、Intel XDK、Famo.us和Construct2等等，未来的Cordova 4.0也计划集成该Web引擎。不过比较遗憾的是，由于iOS的限制（iOS不允许应用使用使用除iOS UIWebView之外第三方的JIT--即时编译引擎），目前Crosswalk也没有办法提供直接的支持，但这也许会随着HTML5更广泛的进入移动市场而发生改变。

### 总结

现在的HTML5 App（加上API扩展）已经可以胜任很多事了，比如教育类应用，休闲游戏等等。不过对于那些实时性要求比较高的、计算量大的（比如涉及大量的元素绘制，或并行计算等）、复杂的3D游戏，多人在线游戏/应用等还有不少差距。另外，工具方面，如何能够更高效的调试/开发/性能内存调优 HTML5应用也是另外一个需要提高的地方。不过，这些方面也在不断的演进。相信不久的将来，HTML5终会成为主流移动开发平台。

### 作者介绍

余枝强目前是英特尔开源技术中心的软件技术经理。 主要负责HTML5 引擎 – Crosswalk 在安卓平台的开发，以及一些其他和Web有关的新兴技术的研发工作（如HTML5 并行技术, 3D Camera等）。他坚信Web是未来， 也非常希望和大家一起努力，让这个未来能够更快更好的到来。

本文转自http://www.infoq.com/cn/articles/html5-crosswalk
