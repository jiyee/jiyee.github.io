---
title: Functional Reactive Programming On iOS
date: 2015-10-12
---

[Ash Furrow](https://github.com/ashfurrow)

ReactiveCocoa developed by Justin Spahr-Summers, Josh Abernathy, Dave Lee

<!---more--->

Declarative Programming vs. Command Programming

函数本身是一个对象，并且是所有类对象中的一等公民。

函数式编程中，对于同样的输入，一个函数始终会给出同样的输出，不存在「可变的状态」。

更多地关注任务是什么，要达成什么目标。

核心的三个函数：
* map
* reduce
* filter

Stream 是 value 的序列化的抽象，你可以认为流就像一条水管，而值就是流淌在水管里的水，值从管道的一端流入，从另一端流出。
当值从管道的另一端流出的时候，我们可以读取过去所有的值，甚至是刚刚进入管道的值（即当前值）。

注意，跟数组一样，流不能包含 **nil** 元素
NSArray 以 nil 作为结束标示，Stream 也一样

Sequence 默认情况下是被动加载的，是 pull-driven 的，在他们被生成的时候就会提供确切的值

Signal 是另一种类型的流，是 push-driven 的，新的值能够通过管道发布但不能像 pull-driven 一样在管道中获取，他们所抽象出来的的数据会在未来的某个时间传送过来。

push-driven： 在创建信号的时候，信号不会被立即赋值，之后才会被赋值（例如，网络请求，用户输入）

pull-driven： 在创建信号的同时序列中的值就会被确定下来，我们可以从流中一个个地查询值。

Signal 发送三种类型值：
- next
- error
- completed

一个事件响应中，一个 Signal 发送了一个 error value 或者一个 completed value 后，就不会再发送任何其他爱的 value。
错误或者成功将只会发送其中一个，绝不会有两个同时发送的情况。

UIKit 的控件都包含一套 Signal 选择器

`RAC()` 宏需要两个参数，对象及其这个对象的某个个属性的 keyPath

指令，RACCommand 类的代表，创建并订阅动作的信号响应。
指令通常是 UI 驱动的，比如用户点击。指令也可以通过信号自动禁用。

UIControl+RACCommand
rac_command 属性

RACSubject 是一个有趣的信号类型。它是一个可变的状态，可以主动发送新值的信号。出于这个原因，除非特殊情况，不推荐使用。

冷信号，除非有人订阅他们，不然他们是不会启动并发送的。
每增加一个订阅，他们都会重复的多发送一个信号。（通过 RACMulticastConnection 改善）

Multicast，用于多个订阅者共享一个信号。
信号的多播连接订阅，当他传送一个新值的时候，是通过公共频道来传送给信号的。只要你喜欢，你可以随意订阅这个信号，但这个信号在订阅相关的操作上有且仅会执行一次，不再像以前那样增加一个订阅者，这个信号上就执行一次订阅相关的操作。

Multicast Subscriptions vs. Multicast Connection

RACReplaySubject，可以确保他背后的 Subject 只会被订阅一次，避免执行重复的操作。
RACReplaySubject 将会缓存这个订阅的值，并将其转发给新的订阅者。

`-configurePhotoModel:withDictionary:` 命名方式

RACObserver 返回一个带属性值的信号，无论这个属性值怎么变都会及时得通过信号反馈出来。

Shadow Variable

RACDisposeable

`-setKeyPath:onObject` 是 RACSignal 的方法，绑定最新的信号值给对象属性

`-perpareForReuse` 方法里销毁 RACDisposeable

RACDelegateProxy，代理 Delegate 协议的方法实现。

`rac_signalForSelector`，当对象 selector 被调用时发送新值的信号。

catch 方法处理方式：它允许无错误值的信号穿透它，仅在信号有错误事件发生时才会调用它的block并发送其在发生错误时的返回值。

如果不用 `RAC()` 宏：
- 有状态：RACDisposable 属性
- 手动处理 RACDisposable 的生命周期

RACObeserve 宏参数的设置大有不同，keyPath 设置的不同，导致不同层级的属性变化会被触发信号发送。

NSURLConnection 也同样有扩展，直接使用方法，返回的 RACTuple 包含了请求响应和数据，同时切回到主线程。

publish 方法返回一个 RACMulticastConnection，当信号连接上时，他将订阅该接收信号。多播保证了网络请求只会执行一次，但是它的结果被多播。

使用 reduceEach: 代替 map: 方法，来处理返回的 RACTuple 对象

将监控数据模型（Model）的变化，并将这个变化映射到视图模型（View Model）的属性上，执行任意必要的业务逻辑。
map 方法中执行必要的转换

didBecomActiveSignal 方法，在异步方法里能控制判断是否冷信号被激活（即异步回调被执行，数据返回）

该视图模型不会与用户交互的接口类有任何交互，不会去引用任何视图类。

冷信号，

热信号，