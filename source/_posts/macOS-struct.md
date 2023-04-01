---
title: macOS 结构简介
date: 2018-09-13 22:59:00
categories: 
- 开源软件
tags: 
- Unix
- macOS
- 开源
---
很多朋友虽然知道 macOS ，然而并不是很了解其中的组成，本文将从底层开始，依次介绍macOS的几个重要组成部分。

说到macOS （Mac OS X、OS X），我们不能不提到它的前身 —— NeXTSTEP。



**前身**

1985 年，乔布斯在内部斗争失败，离开苹果创建了NeXT，在NeXT 中，他的团队基于Mach 和BSD 创建了一款类Unix 的面向对象的操作系统——NeXTSTEP，1996 年乔布斯回归苹果之后，这款先进于Classic Mac OS 的操作系统也随之来到了苹果，最终取代了比较原始的Classic Mac OS。

<!--more-->

**Darwin Operarting System**

Darwin 是macOS 的基础部分（或者称为Core OS），它也是一款开放源代码的类Unix 操作系统。它大体由两部分组成：XNU 内核和Unix 工具。

由于开放源代码的特性，所以一些组织正在利用苹果释出的Darwin 源码进行二次开发，比如PureDarwin 项目。



**XNU**

我们通常会说macOS 的内核是 “Darwin”，其实这是一个不严谨的说法，因为Darwin 不只包含内核，还包括其他东西。严格来说macOS 的内核是 XNU。 

说到这里，插一句题外话，苹果有一个自相矛盾的地方，虽然macOS 已经通过Unix 认证，然而XNU 的全称和 GNU 格式一样，是XNU’s not Unix，顾名思义，XNU 不是 Unix。

XNU是macOS 的核心部分，它是一款结合了微内核与宏内核特性的混合内核，它包括三个部分：Mach、BSD 和I/O Kit。



**Mach**

Mach原来是一款微内核，XNU 中的Mach 来自于OSFMK 7.3 （Open Software Foundation Mach Kernel)）它负责CPU 调度、内存保护等功能。它是macOS 内核中**最重要的部分**，**XNU 中大部分代码来自于它，而且macOS 中的可执行文件也是mach-o 格式**。

 

**BSD**

XNU中包含一个经过修改的BSD，它负责进程管理、Unix 文件权限、网络堆栈、虚拟文件系统、POSIX 兼容。macOS 之所以符合单一Unix 规范，也正是因为如此。

 

**I/O Kit**

I/O Kit 是XNU 内核中的开源框架，可帮助开发人员为Apple 的macOS 和iOS操作系统编写设备驱动程序代码。I/O Kit 框架由NeXTSTEP 的DriverKit 演变而来，与Mac OS 9 的设备驱动程序框架或BSD 的没有任何相似之处。

 

**命令行工具**

除了内核以外，Darwin 还包括一些Unix 工具，这些Unix 工具一些是Apple 开发，一些来自于第三方，比如FreeBSD Project、GNU Project、Apache。

 

**这里说一说它的初始化程序 launchd。**

Launchd 由苹果开发，它是一款统一服务管理框架，用于启动，停止和管理macOS中的守护进程，应用程序，进程和脚本。由于它支持多线程，所以它比传统的Unix初始化程序SysVinit 要高，launchd 同时正在被移植到 FreeBSD 平台，它的设计思想也被 systemd 所借鉴，后者成为目前Linux 发行版中的主流系统初始化程序。



**Core Foundation**

Core Foundation（也称为CF）是macOS和iOS中的C应用程序编程接口（API），是低级例程和包装函数的混合。

 

**Quartz**

macOS 毕竟是类Unix 操作系统，类Unix 操作系统想要进行图形化操作，必须要有一个图形框架，在Linux 上我们有X11 ，有Wayland，在macOS 中，我们有Quartz。它是一款基于PDF 技术的图形框架。

作为一个类unix，不兼容X11是不可能的，如果你想在macOS 中运行X11 应用，也可以，有个开源项目叫XQuartz 了解一下。

 

**Cocoa**

Cocoa是苹果公司为Mac OS X所创建的原生面向对象的API，是Mac OS X上五大API之一（其它四个是Carbon、POSIX、X11和Java）。

苹果的面向对象开发框架，用来生成Mac OS X 的应用程序。主要的开发语言为Objective-c, 一个c 的超集。Cocoa 开始于1989年9月上市的NeXTSTEP 1.0，当时没有 Foundation 框架，只有动态运行库， 称为kit,最重要的是AppKit. 1993 年NeXTSTEP 3.1 被移植到了Intel, Sparc, HP 的平台上，Foundation 首次被加入，同时Sun 和NeXT 合作开发OpenStep 也可以运行在Windows 系统上VCV。

据说Cocoa API 里面到现在还有不少NS 开头的API，何为NS？NS者，NeXTSTEP 也。



**Aqua UI**

macOS 的桌面环境，类似Linux 中的 GNOME。

不过，不是所有Mac OS X 都是 Aqua UI，在Mac OS X 早期测试版 Rhapsoy 中，用的还是经典的Classic Mac OS 界面。

好了，macOS 的一些重要部件就介绍完了，感谢大家的阅读。

 


部分资料来自百度百科 Cocoa 词条和英文维基百科 macOS Darwin launchd 条目，本文的撰写也得到了一些朋友的帮助，在此表示感谢。
