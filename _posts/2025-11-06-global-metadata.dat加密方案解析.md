---
layout: post
title: "global-metadata.dat加密方案解析"
date: 2025-11-06 16:30:00 +0800
categories: 游戏安全
tags: unity  il2cpp  global-metadata.dat
---

Unity引擎在 IL2CPP 模式下，可以将游戏 C# 代码转换为 C++ 代码，然后编译为各平台 Native 代码，从而在一定程度上提高逆向/反编译难度。但IL2CPP并非没有弱点，市面上出现了不少针对性逆向分析工具，如 IL2cppDumper。<!-- more -->  

首先，我们来了解一下，IL2CPP 是如何被破解的。在 Unity IL2CPP 模式下，游戏逻辑是以 Native 代码运行, 但依然存在 C# 某些语言特性(如GC、反射)，会将所有的 C# 中的类名/属性名/字符串等信息记录在 global-metadata.dat 文件中，IL2CPP 启动时会从中读取所需要的信息。

![315_21](/assets/res/202103/IL2CPP原理.png)  
IL2CPP.so 与 global-metadata.dat关系

正是这一机制为外挂制作与游戏破解带来了便利。破解者利用IL2cppDumper工具，只需将游戏包内的libil2cpp.so 与 global-metadata.dat 文件，拖入input文件夹，运行一个命令行，即可完成逆向分析。

![315_21](/assets/res/202103/IL2CPP未加密.gif)  
在未加密情况下 IL2cppDumper 可以解析出 .cs / .json 等文件

随后，破解者可以将解析出的 dump.cs 文件拖入 Visual Studio 解析工具中，直接分析出源码：

![315_21](/assets/res/202103/逆向分析源码.png)  
使用 Visual Studio 可以解析出 .cs 文件中的代码

这样一来，游戏在破解者眼里无异于“裸奔”。破解者可以利用分析出来的代码逻辑制作各类功能的外挂，甚至制作破解版，会严重破坏游戏公平性，造成正常付费玩家大量流失，厂商收益直接受损等负面影响。

所以，对游戏 libil2cpp.so 与 global-metadata.dat 文件进行加密是十分有必要的。基于上述情况，FairGuard研发了一套完善的解决方案，支持Android/iOS/PC/鸿蒙NEXT/Mac多端，已接入多款热门游戏并验证了出色的保护能力，主要包含以下功能：

**■ global-metadata.dat 文件加密**

加密 global-metadata.dat 文件，同时做到对开发者透明。开发者只需使用加固工具运行一条命令行，即可实现加密，不需要上传额外文件。

**■ libil2cpp.so 加壳**

由于 IL2cppDumper 需要依赖 libil2cpp.so 对应 global-metadata.dat 文件中的字符串地址，所以对 libil2cpp.so 做深度加密非常有必要。

FairGuard独创的无导出/无导入函数SO加壳方案，对 il2cpp 进行加壳。效果如下所示：

![315_21](/assets/res/202103/so加壳效果.png)  
FairGuard加壳后il2cpp.so，无导入/导出函数

在使用加密方案后，即使从内存中 dump 出来 libil2cpp.so , 依然不会被 IL2cppDumper 正常识别，效果如下图：

![315_21](/assets/res/202103/IL2CPP加密后.gif)  
加密后IL2cppDumper无法解析

此外，FairGuard还提供反外挂、防破解、资源加密等一系列功能，与引擎加固功能紧密耦合，游戏保护效果更上一层楼。

**丨结语**  

FairGuard作为专注于游戏安全领域的第三方服务商，致力于帮助游戏公司解决外挂和破解问题，为游戏提供深度一体化的加密保护方案。  

目前产品已经被FunPlus、三七互娱、恺英网络、心动、杭州电魂等多家头部公司采用，接入600+款热门游戏。欢迎访问 [www.fair-guard.com](https://www.fair-guard.com) 了解试用。    

产品咨询联系方式：  

QQ：2079128588  

微信：fairguard01  

技术交流QQ群：1105310296  