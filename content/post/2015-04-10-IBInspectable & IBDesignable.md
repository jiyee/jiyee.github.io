---
title: 'IBInspectable & IBDesignable'
date: 2015-04-10
---

`TARGET_INTERFACE_BUILDER`，预编译宏，用于判断是否在IB

```objectivec
	- (void)connectToMyServer {
	#if !TARGET_INTERFACE_BUILDER
	   // connect to the server
	#else
	   // don't connect; instead, draw my custom view
	#endif
	}
```

`IB_DESIGNABLE`，在UIView自定义类头部，将会实时绘制UIView到Interface Builder canvas。（示例是重载了drawRect方法实现的）

`IBInspectable`，在@property属性指定，将能够在IB里控制参数变化，支持的类型包括：**boolean, integer or floating point number, string, localized string, rectangle, point, size, color, range, and nil.**

局部调试，不需要启动App，还能够指定断点（有点意思）
Editor \> Debug Selected Views

由于在 Interface Builder 中呈现自定义视图不会有应用程序的完整上下文，你可能需要生成模拟数据以便显示，这里有两个地方可以处理：

- prepareForInterfaceBuilder()：此方法与你代码的其余部分一起编译，但只有当视图正在准备在 Interface Builder 显示时执行。

- TARGET_INTERFACE_BUILDER：#if TARGET_INTERFACE_BUILDER 预处理宏在 Objective-C 或 Swift 下都是工作的，它会视情况编译正确代码：

[http://nshipster.cn/ibinspectable-ibdesignable/](http://nshipster.cn/ibinspectable-ibdesignable/)