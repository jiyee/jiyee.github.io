---
title: 为 ASUS-AC68U-Merlin 固件交叉编译 shadowsocks-libev
date: 2015-01-24
---

开篇向 [Merlin固件](https://github.com/RMerl/asuswrt-merlin "ASUS-AC68U-Merlin Firewarm")及[Wiki](https://github.com/RMerl/asuswrt-merlin/wiki "Merlin Firewarm Wiki") 致敬，向 tengattack 写下的 《[为R6300v2新固件交叉编译shadowsocks-libev](https://bless.moe/blog/po/1027 "为R6300v2新固件交叉编译shadowsocks-libev")》 一文致敬。

## 背景
本文基本参照了以上两篇文章，经过两个漫漫长夜，终于鼓捣出能够在 ASUS-AC68U Merlin 固件上运行的 shadowsocks-libev。

上周刚买了华硕 AC68U 路由器，看中的就是强大的配置和能够足够鼓捣的空间。但是，本人是个前端开发工程师，目前正在朝 iOS 开发工程师转行，对于怎么样编译固件，编译固件应用，甚是无知。

什么交叉编译，ARM 平台，编译器，基本库等等内容，基本处于很无知的阶段，虽然也用 Mac 和 Linux 平台，但是对于 C/C++ 环境里的一切了解甚少。

## 曲折的过程
好吧，那既然闪闪亮的路由器已经到家了，翻越长城的欲望怒不可止。心想一定要搞到一个能够运行在路由器上的 Shadowsocks。一路 Google，发现根本没有人做过这件事，基本没可能直接拿现成的，看来只能自己编译了。之前看到了 Merlin 源代码里有一个[README.TXT](https://github.com/RMerl/asuswrt-merlin/blob/master/README.TXT)，有写怎么样配置编译环境和怎么编译固件，但是后来发现这个文档是基于 MIPS 平台，而 AC68U 却是 ARM 平台，文档过时了。。。另外，找到 Merlin 固件也能够支持 [Entware](https://github.com/RMerl/asuswrt-merlin/wiki/Entware)，但是却又说：
> Note that Entware is only available on the MIPS-based routers. This means the RT-AC56U and RT-AC68U are not supported.

好吧，那继续 Google 和翻论坛帖子，国内恩山论坛上搜索 AC68U 帖子数还很少，国外 [smallnetbuild](http://forums.smallnetbuilder.com) 上也不多，估计是 AC68U 的用户基数比较少，而愿意折腾的同学会选择性价比更高的其他型号。

最后，不知道哪儿看到 [一篇文章](http://wiki.openwrt.org/doc/devel/crosscompile) 说 OpenWRT 固件里可以把需要编译的应用放到 packages 里，能就基本 SDK 编译了。虽然这段话到现在还不知道怎么理解，但至少提供了一个思路，那就是先编译一个编译 Merlin 固件看看。

编译固件需要编译平台，看 Merlin 作者推荐 Ubuntu，我就用 Vagrant 搭建了 Ubuntu 环境，下载了 Ubuntu 14.04 64bit 的 box（64bit还是给后面工作埋下了坑），成功加载运行，链接本地 `/Users/jiyee/Vagrant` 目录到虚拟机的 `/vagrant` 目录。Merlin 固件源代码之前已经 clone（好大，有4Gb），就放在 `/Users/jiyee/Vagrant` 目录。这一步走的比较顺利。

然后，按照 [Merlin Wiki](https://github.com/RMerl/asuswrt-merlin/wiki/Compile-Firmware-from-source-using-Ubuntu) 里的配置编译环境的步骤，一步步设置，通过 apt-get 安装一堆编译工具，配置 `$PATH` ，最后执行 `make rt-ac68u` 。看 [一篇文章](http://see.sl088.com/wiki/%E7%BC%96%E8%AF%91Openwrt%E5%9B%BA%E4%BB%B6) 说固件编译很慢，就合上电脑睡觉了。第二天醒来一看，有报错，很正常，”编译固件有报错很正常”，心想。但是，错误提示一大堆，基本也看不懂。想到论坛上问问作者，后来一想环境不一样，估计问了也白问，还得注册论坛，写一篇英文帖子去描述，粘贴一堆编译信息，想想就累，遂放弃了。不过，至少我配置好了编译环境，小开心。退一步说，即便能够编译出固件我也不知道下一步该怎么办。

这一下打击让我歇了两天，两天之后再次兴起想到这件事，翻出原来 Google 来的那些博客文档，看到了 tengattack 写的 《[为R6300v2新固件交叉编译shadowsocks-libev](https://bless.moe/blog/po/1027 "为R6300v2新固件交叉编译shadowsocks-libev")》 一文，虽然这篇文章之前也草草看过，没看明白，所以决定这次认真看看。

在配置环境的过程中，我了解到了 [uclibc](http://www.uclibc.org/) 编译环境，它是嵌入式平台的编译库，看到这篇文章花很大力气从 uclibc 编译环境迁移到 musl。google 到 [musl](http://www.musl-libc.org/)，看到主页上有一个链接 [《See how musl compares to other major libcs.》](http://www.etalabs.net/compare_libcs.html) ，一眼就看到了 glibc，那不是 GNU 的 C Library 么（我还是知道一点的），推测 uclibc，musl，glibc 都是一回事，只是 C Library 在不同平台上的实现或是同一平台上的不同实现。心想，我都不用 uclibc 迁移到 musl，因为 Merlin 固件就是在 uclibc 环境下编译的，省事。

作者也没提编译固件的事，直接上来就编译程序了，看来有点眉目了。

那好吧，我明白了交叉编译的最基本概念，就是在一个平台编译另一个平台能运行的程序。就写了一个最基本的C代码，`hello.c`。

```c
	#include <stdio.h>
	
	int main()
	{
	    printf("Hello World!\n");
	    return 0;
	}
```
 
运行 `gcc hello.c -o hello` ，编程成功，运行 `./hello` ，成功打出了 `Hello World!`。这是 C 语言学习的第一步，而我却拿来验证了编译器是否能成功执行。

然后运行 `arm-uclibc-linux-2.6.36-gcc hello.c -o hello` ，报错，傻眼。
> /opt/brcm-arm/bin/../libexec/gcc/arm-brcm-linux-uclibcgnueabi/4.5.3/cc1: error while loading shared libraries: libmpc.so.2: cannot open shared object file: No such file or directory

Google 一番之后，说有让执行 `ldd /opt/brcm-arm/libexec/gcc/arm-brcm-linux-uclibcgnueabi/4.5.3/cc1` 看看动态链接库是否完整，一看果然有 Not Found。

>    linux-gate.so.1 =\>  (0xf77bf000)
>    libmpc.so.2 =\> not found
>    libmpfr.so.4 =\> not found
>    libgmp.so.10 =\> not found
>    libdl.so.2 =\> /lib/i386-linux-gnu/libdl.so.2 (0xf77aa000)
>    libelf.so.1 =\> /usr/lib/i386-linux-gnu/libelf.so.1 (0xf7792000)
>    libc.so.6 =\> /lib/i386-linux-gnu/libc.so.6 (0xf75e3000)
>    /lib/ld-linux.so.2 (0xf77c0000)

有三个 so 包没有能链接，在 `/usr/lib/i386-linux-gnu/` 目录里果然没有 `libmpc.so.2` 等文件。再一番 Google，意思是 [装个32位链接库看看](http://unix.stackexchange.com/questions/119798/mint-correct-way-to-install-lib-i386-linux-gnu-libgmp-so-3)，执行 `apt-get install ia32-libs`，安装错误。。。漫漫过程中一个库网络链接好像失效了，安装失败。这个时候在 StackOverflow 上看到了 [一个帖子](http://stackoverflow.com/questions/19625451/cc1-error-while-loading-shared-libraries-libmpc-so-2-cannot-open-shared-objec) 说，是否正确设置了 **LD\_LIBRARY\_PATH** 环境变量。心想，Merlin 固件的 Wiki 里没有提到呀，什么东西呀，那先 `echo $LD_LIBRARY_PATH` 看看，一 echo 为空，再在 `/opt/brcm-arm` 目录里找找，也有一个 `lib` 目录，里面就有那些缺失的文件。那就设置 `export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/opt/brcm-arm/lib:/usr/local/lib:/usr/lib`。再执行 `ldd /opt/brcm-arm/libexec/gcc/arm-brcm-linux-uclibcgnueabi/4.5.3/cc1 `，内容发生了变化，都能找到了。

>    linux-gate.so.1 =\>  (0xf76f4000)
>    libmpc.so.2 =\> /opt/brcm-arm/lib/libmpc.so.2 (0xf76de000)
>    libmpfr.so.4 =\> /opt/brcm-arm/lib/libmpfr.so.4 (0xf768e000)
>    libgmp.so.10 =\> /opt/brcm-arm/lib/libgmp.so.10 (0xf7631000)
>    libdl.so.2 =\> /lib/i386-linux-gnu/libdl.so.2 (0xf7620000)
>    libelf.so.1 =\> /usr/lib/i386-linux-gnu/libelf.so.1 (0xf7608000)
>    libc.so.6 =\> /lib/i386-linux-gnu/libc.so.6 (0xf7459000)
>    libm.so.6 =\> /lib/i386-linux-gnu/libm.so.6 (0xf7412000)
>    /lib/ld-linux.so.2 (0xf76f5000)

再次执行 `arm-uclibc-linux-2.6.36-gcc hello.c -o hello`，编译成功，执行 `./hello`，报错
> bash: ./hello: cannot execute binary file: Exec format error

欣喜，貌似成功了。那把编译出来的 hello 文件 scp 到路由器上再执行，成功打印了 `Hello World!`，哇哦，看来离成功不远了。

因为 Merlin 固件自带里 OpenSSL，省下一大把精力。

再看了一遍那篇文章，照猫画虎的写下了以下编译命令：

```shell
	CC=arm-uclibc-linux-2.6.36-gcc CXX=arm-uclibc-linux-2.6.36-g++ AR=arm-uclibc-linux-2.6.36-ar RANLIB=arm-uclibc-linux-2.6.36-ranlib ./configure --prefix=/vargrant/shadowsocks-libev --host=arm-uclibc-linux
	make && make install
```

`/vargrant/shadowsocks-libev/src` 目录下出现了4个可执行文件。成功了。。。我哭啊，赶紧再 scp 到路由器，执行 `./ss-local -s server_ip -p 8080 -l 7070 -k password -v -m aes-256-cfb`，又成功了。。。在本地配置 SOCKS5 代理，嗖嗖地翻出了万里长城。


## 后记

哇哈哈，洋洋洒洒写了这么多，虽然实质内容没多少，也基本都是别人的，但是对于一个不懂交叉编译甚至是编译原理的同学来说成功能够搞定这一切，还是很值得欣慰的。我把其中经历的每一步都详实地写下来，把其中参考过的文档都附上链接地址，只是希望能够给像我这样的同学作一点参考，在茫茫 Google 搜索结果里找到一点点希望。

其实，很多文章在第一次 Google 的时候会被忽略，写太多了，看不懂。第二次 Google 的时候会被扫过，挑重点看看。第n次 Google 的会被关注，醍醐灌顶，深深受益。因为你每一次都站上了不同的高度，直到你够到了作者的高度。最后，当你自己想写下什么的时候，他们会被感谢，因为没有他们，你就无法完成这一切。

最后，为了 Google，感谢 Google。
