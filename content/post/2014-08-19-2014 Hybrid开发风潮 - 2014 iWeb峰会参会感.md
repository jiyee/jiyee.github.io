---
title: 2014 Hybrid开发风潮 - 2014 iWeb峰会参会感
date: 2014-08-19
---

上周末参加了 2014 年度的 iWeb 峰会，那叫一个人山人海啊。不过跟前几年有点不一样的是，今年似乎正刮一阵 Hybrid 开发风，主会场和工具专场连着介绍了三款不同的 Hybrid 开发框架，[Native.js](https://github.com/dcloudio)，[AppCan](http://www.appcan.cn)，[Intel XDK](https://software.intel.com/en-us/html5/tools)，各自有不同的思路和实现，给 Web 开发者们提供了不同的 App 开发平台和能力，那就一个个分别说说。

### Native.js

实质上属于 [HTML5+](http://www.html5plus.org/) 规范和 HBuilder 的结合实现， HTML5+ 就没什么好说的，据说是国内组织搞的（不太清楚，感觉就是 DCloud 牵头搞的），应用上跟 PhoneGap 类似。 Native.js 属于 HTML5+ 规范未实现的原生 API 部分的 Proxy ，是不是可以理解为那些规范里的实现都是通过 Native.js 实现的，就是暴露了原生 API 封装实现给了开发者，看难度好像有点大，需要根据不同平台调用原生 API 。那就要求开发者理解那些原生 API ，思路上跟 Titanium 的 Widget 类似，但是实现上选择了 JS Bridge 方式，我认为不是很好的一个方向，有点噱头的意思。那在它的平台上就只能希望 HTML5+ 规范的部分能实现的更加完整和全面。

关于 Native.js 的实现，我的猜测是大量的使用了反射来将 JS 转为 Java 或者 Objective-C ，性能上是很大的考验。

另外， HBuilder 是基于 Aptana 开发的，更加倾向于小清新，会场上也真的已经有了不少实际用户，让我大吃一惊。

关于 HBuilder 的介绍参看：[《近匠》HBuilder：如何用JS调用几十万原生API？](http://www.csdn.net/article/2014-04-11/2819266-jinjiang-with-hbuilder)

Dcloud.io 官网上有关于 Native.js 的[PDF文档](http://download.dcloud.net.cn/HTML5+%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91-Native.js.pdf)

### AppCan
国内比较成熟的 Hybrid 开发平台，开发框架涵盖了丰富的自定义 API ，商业模式也比较清晰，但是，相对来说比较封闭，更加适合政府部门，我猜的。

具体的就不予置评了，请参看[AppCan官网](http://www.appcan.cn)

### Intel XDK
Intel XDK 框架真是第一次听说，之前孤陋寡闻了。 Intel 居然也加入了 Hybrid 开发阵营，可能真的是像他们的老大张海立所说的这款产品之前一直是国外团队在开发，国外团队也刚开始接手不久。

张海立的演讲很精彩，搜了一下，复旦毕业，言语真的很像上海人，还真有点想投奔他的感觉，哈哈。

至于 Intel XDK 这款产品，那就是站在开源产品之上集大成，相较于前两位更加开放、新潮，集成了包括[Cordova](http://cordova.apache.org)，[Ripple](http://ripple.incubator.apache.org)，[Brackets](http://brackets.io)， V8 ，还有一些开源的 UI 框架和开放的 Service 服务，将设计、开发、测试、编译、分发集成设计出一套完整的开发方案， IDE 级别。

出彩的 [Crosswalk](https://crosswalk-project.org)，看了下官网介绍，似乎是 Cordova 升级版，以至于每次出现 XDK 必出现 Crosswalk ，躲都躲不过。

[XDK Slide](http://slides.com/zhanghaili/intelxdk#/)，瞧人家多潮，都用上 slides 了。。。

![iWeb 峰会主会场](/images/iweb.jpg)
