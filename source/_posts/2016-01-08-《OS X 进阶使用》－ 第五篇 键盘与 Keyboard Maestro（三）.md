---
title: 《OS X 进阶使用》－ 第五篇 键盘与 Keyboard Maestro（三）
date: 2016-01-08
---

终于讲到键盘这个主题的正文了，前面临时插入的两篇文章，无论从选题是重要性来讲都更为重要，反倒是显得讲不讲 Keyboard Maestro 无所谓了。

但是，不能辜负这个随意起的标题，延续下一个主题带着一个典型应用的方式。

[Keyboard Maestro](http://keyboardmaestro.com) 其实是一款自动化软件，属于 OS X 典型的大而全的专业应用。只不过他的触发器一般都是快捷键罢了，至于按下快捷键之后能做些什么，看看他定义了哪些能够实现的[功能](https://www.keyboardmaestro.com/documentation/7/features.html)，例如启动软件、移动窗体、插入文本、模拟点击、管理剪切板，文件操作，甚至能够自定义流程和宏。

> "The only limit to Keyboard Maestro is your imagination!"

这就是它的口号，对得起它的价格和它能做的事。

但是，我回想了下我使用它的场景，大概用到的功能不到 5％ 吧。用的最多的就是按下快捷键启动或者激活某个特定的应用，以至于我经常将它遗忘。

对于程序员来说，如果想更直观地设定自动化流程，我更加推荐 [Hammerspoon](http://www.hammerspoon.org) 这款应用，免费且支持 Lua 脚本（ Lua 很简单，分分钟就能学会），通过 Lua 脚本而不是 GUI 的方式来更为精细化地控制系统环境，而且它还支持一些更加底层的事件和行为，例如 电池、WiFi、USB 设备等。他还能跟第三篇里提到 Karabiner 组合，通过快捷键和 URL Schema 方式触发自定义宏。

另外，如果大家对自定义手势感兴趣，可以参考 [BetterTouchTool](http://www.boastr.net/) 和 [Jitouch 2](https://www.jitouch.com/) 这两款应用，有非常不错的自定义手势和功能支持。

最后想说的是，其实这些应用都是需要从自身的需求出发，根据实际情况去配置。理想的情况是，当你发现在每日的工作中经常性地重复某一个动作，以至于你都注意不到的时候，这时候回过头来想想是不是可以优化下自己的工作方式。到这个时候，这些自动化软件就可以帮上你忙的了。

最后，忘了 OS X 其实自带的 Automator 功能也很不错，只不过之前只有 GUI 方式 和 AppleScript 脚本支持，现在支持 JavaScript 脚本，只不过几乎没有用过，不再评述。

如果想 iOS 上折腾，可以找找一款叫 [workflows](https://workflow.is) 的自动化应用。

Windows 用户可以看看 [AutoHotkey](https://www.autohotkey.com) 这款软件，能实现类似的功能。