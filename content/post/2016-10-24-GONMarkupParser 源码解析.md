---
title: GONMarkupParser 源码解析
date: 2016-10-24
---

https://github.com/nicolasgoutaland/GONMarkupParser
这个项目源代码质量不错，代码结构简洁，思路清晰。
 
目前，支持的标签和属性列表，《HTML标签支持规范》。
 
## HTML 解析流程草稿

![](/images/gon-markup-parser-1.jpg)
![](/images/gon-markup-parser-2.jpg)
![](/images/gon-markup-parser-3.jpg)

 
## 总结

很好的区分了 defaultConfiguration，defaultAttributes，stringAttributes 等概念，使用注册方式来实现标签扩展，规范了自定义 Markup 注册和解析流程，与正则解析流程较好融合，通过 context 实现栈内数据共享。
GONMarkup 标签解析分为三个步骤，前置文本替换，标签内文本更新，后置文本替换。
 
## 使用方式
如果有 Link 且想点击，使用 UITextView 或 TTTAttributedLabel。
如果想自定义标签，继承 GONMarkup 类，按需求实现不同阶段的文本替换函数。
如果采用 HTML 标签形式替代现有的样式写法，一是可以统一样式实现，二是实现服务端可配，三是简化客户端样式布局代码。
