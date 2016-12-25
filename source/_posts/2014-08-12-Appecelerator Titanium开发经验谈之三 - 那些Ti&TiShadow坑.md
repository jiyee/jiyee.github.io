---
title: Appecelerator Titanium开发经验谈之三 - 那些Ti&TiShadow坑 #一个前端工程师的App之路
date: 2014-08-12
---

- 图片素材放在 app/assets/images/ 目录，但是在引用的时候 TiShadow 环境里可以直接写 pic.png，但是在 Android 编译环境下就得写成 /images/pic.png

- lib 放在 app/assets/lib/ 目录

- TiShadow 环境里 CommonJS 写法可以写成 `exports = {methodName: methodName}`，但是在 Android 编译环境下就得写成 `exports.methodName = methodName`

**PS：** 上述坑还是吃了不了解 Android 编译过程和 TiShadow 运行时环境的亏，看来即便搞 Hybrid App 开发，不懂 Native App 开发原理，还是搞不定。

