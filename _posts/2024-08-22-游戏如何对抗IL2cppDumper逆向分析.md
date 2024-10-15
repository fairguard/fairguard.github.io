---
layout: post
title: "游戏如何对抗IL2cppDumper逆向分析"
date: 2024-08-22 16:26:10 +0800
categories: 游戏安全
tags: Unity  IL2cppDumper  逆向分析
---

众所周知，Unity引擎中有两种脚本编译器，分别是 Mono 和 IL2CPP 。相较于Mono，IL2CPP 具备执行效率高、跨平台支持等优势，已被大多数游戏采用。<!-- more -->  

IL2CPP 模式下，可以将游戏 C# 代码转换为 C++ 代码，然后编译为各平台 Native 代码。Native 代码提高了逆向/反编译难度，可有效提高外挂开发和游戏破解的门槛。  

![315_21](/assets/res/202103/IL2CPP构建项目自动步骤图1.png)  
▲IL2CPP构建项目自动步骤图  

虽然 Unity IL2CPP 在一定程度上提高了破解门槛，但并非没有弱点，市面上出现了不少针对性逆向分析工具，如 IL2cppDumper。  

本文将着重分析 IL2CPP 模式下，游戏应该如何应对 IL2cppDumper 的逆向分析，并提供行之有效的解决方案。  

首先，我们来了解一下，IL2CPP 是如何被破解的。  

在 Unity IL2CPP 模式下，游戏逻辑是以 Native 代码运行, 但依然存在 C# 某些语言特性(如GC、反射)，会将所有的 C# 中的类名/属性名/字符串等信息记录在 global-metadata.dat 文件中，IL2CPP 启动时会从这个文件读取所需要的信息。  

![315_21](/assets/res/202103/IL2CPP原理.png)  
IL2CPP.so 与 global-metadata.dat关系  

正是这一机制为外挂制作与游戏破解带来了便利。破解者只需将游戏包进行解压，找到 libil2cpp.so 与 global-metadata.dat 文件，拖入input文件夹，运行一个命令行，即可完成逆向分析。  

![315_21](/assets/res/202103/IL2CPP未加密.gif)  
在未加密情况下 IL2cppDumper 可以解析出 .cs / .json 等文件  

随后，可以将解析出的 dump.cs 文件拖入 Visual Studio 解析工具中，直接分析出源码：  

![315_21](/assets/res/202103/逆向分析源码.gif)  
使用 Visual Studio 可以解析出 .cs 文件中的代码  

这样一来，游戏在破解者眼里无异于“裸奔”。破解者可以利用分析出来的代码逻辑制作各类功能的外挂，甚至制作破解版，会造成游戏公平性破坏，正常付费玩家大量流失，厂商收益直接受损等严重影响。  

基于这种情况，FairGuard研发了一套完善的解决方案，支持Android/iOS/PC/鸿蒙多端，目前已接入多款热门游戏并验证了出色的保护能力，主要包含以下功能：  

■ global-metadata.dat 文件加密  

加密 global-metadata.dat 文件，同时做到对开发者透明。开发者只需使用加固工具运行一条命令行，即可实现加密，不需要上传额外文件。  

■ libil2cpp.so 加壳  

由于 IL2cppDumper 需要依赖 libil2cpp.so 对应 global-metadata.dat 文件中的字符串地址，所以对 libil2cpp.so 做深度加密非常有必要。  

FairGuard独创的无导出/无导入函数SO加壳方案，对 il2cpp 进行加壳。效果如下所示：  

![315_21](/assets/res/202103/so加壳效果.png)  
FairGuard加壳后il2cpp.so，无导入/导出函数  

在使用加密方案后，即使从内存中 dump 出来 libil2cpp.so , 依然不会被 IL2cppDumper 正常识别，效果如下图：  

![315_21](/assets/res/202103/IL2CPP加密后.gif)  
加密后IL2cppDumper无法解析  

此外，FairGuard还提供反外挂、防破解、资源加密功能，与引擎加固功能紧密耦合，游戏保护效果更上一层楼。  

反外挂功能  

针对游戏将面临一系列外挂修改风险，FairGuard研发了行为检测方案，搭配300+维度的智能感知系统，可通杀各类外挂及其变种，做到有效防护。  

防破解功能  

采用FairGuard业界独家技术「无API签名校验技术」，从底层出发，对游戏的引擎与代码进行加密处理，可以针对游戏包签名和文件完整性进行多重校验，防止游戏被植入恶意模块、剔除广告等。  

资源加密功能  

FairGuard独家资源加密方案，支持Android、iOS、PC、H5、鸿蒙多端。可为游戏资源提供高强度加密保护，同时具备高兼容性、运行消耗小、性能无影响的特点，并且支持资源在线更新。  


**丨结语**  

FairGuard作为专注于游戏安全领域的第三方服务商，致力于帮助游戏公司解决外挂和破解问题，为游戏提供深度一体化的加密保护方案。  

目前产品已经被FunPlus、三七互娱、恺英网络、心动、杭州电魂等多家头部公司采用，接入500+款热门游戏。欢迎访问 [www.fair-guard.com](https://www.fair-guard.com) 了解试用。    

产品咨询联系方式：  

QQ：2079128588  

微信：fairguard01  

技术交流QQ群：1105310296  