---
title: 《OS X 进阶使用》－ 第三篇 键盘 和 Keyboard Maestro
date: 2016-01-06
---

TL;DR
键盘是程序员每天都要接触的输入设备，很有必要把键盘的好好研究一番。。。
如果不是爱折腾人士，请忽略本文。

## 机械键盘
[机械键盘](https://zh.wikipedia.org/zh/%E6%9C%BA%E6%A2%B0%E9%94%AE%E7%9B%98)可能是每一个程序员都必备的一样外设，无论是 PC 用户还是 Mac 用户。他带来的触感和体验跟普通的薄膜键盘相比存在明显的差异，个人推荐茶轴或者青轴更适合程序员使用。至于具体哪款键盘型号或者不同轴之间的差异，这里面略微有些玄学的意思，乃至于像 HHKB 这样的静电容键盘，我没使用过，就更不好评说了。

如果大家对外设感兴趣，可以看看 [in外设](http://www.inwaishe.com) 这个网站，里面尽是些外设的测评的内容，看了要剁手不要找我。

## 键位
OS X 用户在外接键盘的时候，第一个问题就是在默认情况下 Command 键和 Option 键换了个位置。为了达到一致性体验，建议换回默认键位。方法也比较简单，就是在「设置－键盘－修饰键」里重设实体 Option 键和 Command 键对应的按键即可。

另外，就是个性化的一些改变了，这里稍微展开一点，如有兴趣可以继续参看文章底部参考文档。

### Hyper Key & Seil
古早的键盘拥有更多的修饰键，如 Hyper、Super、Meta，用于状态控制。
![Emacs](http://holywood.be/emacs/images/keyboard-9647.jpg)

现在，通过更改键位的方式可以在现代键盘上模拟出一些修饰键，用于快捷键设置。

在 OS X 里一个叫 [Seil](https://pqrs.org/osx/karabiner/seil.html.en) 的免费软件可以完成键位的 keycode 替换。

对于现代键盘而言，最有用的一点是替换 CapsLock 键。因为 CapsLock 键不经常使用，却又在最容易触及的位置，把 CapsLock 键替换成 Hyper 键或者 Ctrl 键。尤其是对于 Vim 党，很多 Vim 党就是在 Vim 配置把 CapsLock 键替换成 Meta 键使用。

### Karabiner
事情肯定没有这么简单，那替换了之后能有什么用呢？ 
下面就请出另一个免费神器 [Karabiner](https://pqrs.org/osx/karabiner/index.html.en) ，用于键盘的自定义，通过选项方式或者配置文件方式，就能完成各式各样的按键自定义，完全是为爱折腾人士准备。

例如，通过按 Hyper＋Tab 方式切换的 CapsLock 状态，Hyper＋J 映射到 Down，Hyper＋K 映射到 Up，模拟在普通输入框里实现 Vim 按键映射，避免了 OS X 默认的 Emacs 快捷键方式的影响，是不是 Vim 党最爱？

之前，我使用键盘有一个毛病，就是只用左 Shift＋其他键来实现大写或者符号输入，无论是 Shift＋A 还是 Shift＋K等，相信很多人也有同样的问题。后来看到通过 Karabiner 可以设定只允许正确的使用左右 Shift 键才能出来完成输入，例如左 Shift＋A 是无效的按键，右 Shift＋A 才能完成大写字母 A 的输入。很简单的一个配置就能实现，从此，妈妈再也不用担心我傻傻不会用键盘了。

下面附上我一张使用 [WhatPulse](https://whatpulse.org/) 统计按键频率图。
![WhatPulse 键盘按键统计](/images/what-pulse.png)

最后，附一篇我觉得写得最好的关于键盘配置的参考文档：
[现代 Space Cadet](http://ranmocy.me/translation/a-modern-space-cadet/)

Windows 似乎没有类似的软件，有一款 [AutoHotkey](https://www.autohotkey.com/
) 的软件跟下一篇讲的 Keyboard Maestro 类似，大家有兴趣可以了解一下。

还没写到 Keyboard Maestro 就已经这么长，暂时就到这里，更多的内容留待下一篇继续。
