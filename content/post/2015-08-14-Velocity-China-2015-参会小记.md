---
title: Velocity China 2015 参会小记
date: 2015-08-14
---

主要听的主题涵盖：**APM**，**SPDY & HTTP/2.0**，**React & Flux**，**Optimization**

## APM (Application Performance Management)
- 云智慧
- SOASTA
- 听云
- OneAPM
- 性能魔方
- 性能极客
- 野狗
- 阿里云 CAS

APM产品同质化严重，各家差异不明显，用户规模上差异罢了。

## SPDY & HTTP/2.0
http://caniuse.com/#search=http

HTTP/2.0 已经不再遥远，Chrome 已经 v41 就已经支持，即将发布的 iOS 9 将支持。而 SPDY 则有更好的兼容性，尤其在 Android 和 iOS 平台。

只升级协议的基础上，性能提升就在 5%~30%，而如果针对 SSL 进行特定优化，则能有更好的优化。

SPDY 协议主要支持单条 TCP 链路支持并发 HTTP 请求，HTTP 头部压缩，自定义消息格式，服务端推送等特性。

2014以来，大厂们的 App 都已经纷纷采用 SPDY 协议，谁用谁知道。

## React & Flux
这个 Topic 主要由 Yahoo 的**朱凌燕**带来。React 几乎是 2015 年前端最火热的框架，它重新定义了前端框架。面向 Component 开发，采用 JSX 这种在 JavaScript 里写 HTML 方式，并以 VirtualDOM 方式分离了页面元素的表示与实现，进一步提升了性能，提高了代码复用程度，无论是前端性能还是复杂度又提升了一个台阶。

Flux 是单向数据流实现的应用架构，基于 Flux 构建的 React 应用，数据流动保持了一致性。

Yahoo 进一步推动的是前后端同构的 Flux，后端采用 Node.js 实现，一样基于 Flux 单向数据流方式，前后端共享组件和业务逻辑，支持前后端输出，对 SEO 更友好。对于 IE 8 以下浏览器，采用降级方式，保证主体功能可用。

全栈工程师在开发过程中，不需要进行 context 切换。另外，推动了组件化共享。

后续，需要进一步关注 **Relay**， **Netflix Falcor**，**socket.io**，**acss.io**。

 ## Optimization
 ### QQ WebView 性能优化
 QQ WebView页面通过 zip 包方式分发，经历过三个阶段。
 1. CDN 全量更新
 2. cgi + patch 方式动态请求更新包
 3. 通过 **bsdiff / bspatch**，实现增量更新
 
 主要介绍的增量更新方式，客户端包含基线包，增量包更新进行完整性和安全性校验，避免更新包损坏或者被篡改，JS 变量按照变量使用频率进行混淆（为了差分时减少变化），增量包生成在服务器，属于 CPU 高消耗的操作，增量包加载在客户端，属于 CPU 低消耗的操作，采用的是最长公共子序列算法生成差分字符。

另外，为了避免频繁更新造成白页 loading 状态，采用动静分离，按照改动频率区分基线包和更新包。

更新包采用 JSON 格式，包含 Data，JS，CSS 三部分。

数据层面上，基线包从 400Kb 瘦身到 40Kb。

另外，讲了些遇到的坑，包括废弃使用DB，直接采用 LocalStorage；废弃使用 NativeImgCache，直接采用 Last-Modified 方式进行图片资源缓存；H5首页 和 Native 交叉调用请求的合并。

### 手机淘宝性能优化
性能瓶颈：CPU Load 高、连续 I/O，内存对象过多导致频繁 GC、移动网络。
性能监测指标：响应时间、FPS、GC次数、内存占用。

优化方案：
1. 布局优化，避免过多嵌套
2. 本地缓存，建立离线化 View
3. 初始化任务分级，合理并行
4. 懒加载，多模块集中场景
5. 预加载，弱网情况
6. 图片在弱网情况下，采用锐度较高、质量较低的图片，显示效果还可以，弱网情况下，列表页的图直接给详情页使用
7. 图片bitmap对象复用

网络优化：
1. 报文缩减，采用 PB 格式
2. 域名收敛（最初为了在图片并行下载，采用多个图片域名方式）
3. HTTP DNS（就是 7层自建DNS，实现网络优化、控制流量）
4. SPDY 长链接
5. TCP 参数调优
