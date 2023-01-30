---
title: WhatPulse键盘按键数据分析
date: 2015-03-11
---

键盘：Filco Ninja（87键）

分析周期：2014-10-11 ~ 2015-03-09 (138天)

总按键次数：2,863,205

平均每天：20,749

![](/images/what-pulse.png)

![](/images/keys-heatmap.png)

总结看出，Backspace、Space、Enter、Tab 按键高居前列，总体写代码和 Shell 操作相符。 Backspace 数量明显高于其他，说明输入表达的准确性和稳定性不够，内容出现修改的情况突出（就包括现在打这段文字的时候，有意识地意识到自己不停地在按 Backspace ）。当然， Backspace 跟 Escape 之间的比例也没有 rio 说的 10：1 那么悬殊，可能 Shell 老让我等待退出，不停地按 Esc。

 Capslock 键换成了 Hyper 键，但是按键次数上明显少于其他 Ctrl、Alt、Shift 键，WhatPulse 不能统计 Command 键，估计按键次数同样不会少，有点可惜。

相比于左侧的功能键，右侧除了 Shift 之外，Alt 和 Ctrl 几乎少有触及，Shift 键得益于 Karabiner ，强制左侧 ASDF 之类的大写字母必须搭配按右侧的 Shift 键，右侧的 Command 键发挥切换输入法的作用，但是按键次数明显也不及其他功能键，可能切回英文输入法已经集成到 Esc 退出 Vim 插入模式造成的。右侧的 Ctrl 键就可怜了，我几乎想不到使用的场景，可惜了。

F1 到 F12 功能键按键频率同样不高，可能跟之前没有做 iOS 开发，缺乏 IDE 调试有关。（F11 和 F12 属于异常数据，Vim 退出插入模式的时候会触发切换输入法的动作，动作的快捷键映射了 Command+Alt+Ctrl+Shift+F11/F12）

数字键 1 到 4 用于输入法选择和 Tab 切换，5 到 9 的使用频率就相对少很多。

总结下来感受是，我的按键频率相对合理，但是仍然有可优化的空间。下一步考虑如何好好开发右侧功能键、F1 到 F12 键，以及 5-9 之间的数字键的习惯使用。

我很满意我的 Ninja 键盘。

![](/images/filco-ninja-87.jpg)

相关参考：
[历经 199 天得到的数据，退格使用次数高达 13 万次](http://daily.zhihu.com/story/4213946 "历经 199 天得到的数据，退格使用次数高达 13 万次")
