---
layout: post
title: "Zygisk-IL2CppDumper对抗方案"
date: 2023-09-27 16:26:10 +0800
categories: 游戏安全
tags: unity  IL2Cpp  Zygisk-IL2CppDumper
---

众所周知，Unity引擎中有两种脚本编译器，分别是 Mono 和 IL2CPP 。这两种脚本编译器各有优势，同时也存在一些安全性问题，本文将从游戏安全角度对其进行分析并提供对策。<!-- more -->  

Mono 是由跨平台的开源.NET 实现，它允许开发者使用 C# 等编程语言编写游戏逻辑，为游戏开发提供了简单易用的环境和高效的脚本编译速度，得到了一些中小型游戏的青睐。  

在 Mono 模式下，游戏 C# 代码被编译为 IL (中间代码) 并生成 dll 文件，然后将 dll 打进游戏包文件。但由于 IL 非常容易被 ILSpy / .NET Reflector 等专业反编译软件分析逆向，所以在无保护情况下，游戏的安全性极差。  

![315_21](/assets/res/202103/.NETReflector还原出C#文件.png)  

.NET Reflector 几乎可以还原出 C# 文件  

相对于 Mono，IL2CPP 可以将游戏 C# 代码转换为 C++ 代码，然后编译为各平台 Native 代码。  

Native 代码提高了逆向/反编译难度，可有效提高外挂开发和游戏破解的门槛。此外，IL2CPP 还具有执行效率高、内存占用小等优势，已被大多数游戏采用。  

![315_21](/assets/res/202103/IL2CPP构建项目自动步骤图.png)  

IL2CPP构建项目自动步骤图  

虽然 IL2CPP 可以在某种程度上提高游戏的安全性，但并非没有漏洞。在 IL2CPP 模式下，Unity 将 C# 代码中的类名、属性名、字符串等信息记录在 global-metadata.dat 文件，引擎依赖这些数据实现某些 C# 语言特征。  

而 IL2CPP 启动时会从这个文件读取所需要的类名、属性名等信息，正是这一机制给了破解者可乘之机。  

破解者可使用 IL2CPPDumper 解析 global-metadata.dat 文件，并将文件里的类名等字符串信息对应到 Native 代码中去，大幅降低了逆向分析难度。  

![315_21](/assets/res/202103/IL2CPPDumper接收路径.png)  

IL2CPPDumper 接收 libil2cpp.so / global-metadata.dat文件路径  

面对IL2CPPDumper的逆向分析，可采用对 libil2cpp.so 文件、global-metadata.dat 文件进行加密来与之对抗，从而避免游戏代码被分析。  


而 Zygisk-IL2CppDumper 、funil2cpp 等逆向工具的出现，将游戏安全对抗激烈程度抬升到了新高度。  

![315_21](/assets/res/202103/Zygisk-il2cppDumper界面及操作流程.png)  

逆向工具Zygisk-il2cppDumper界面及操作流程  

**Zygisk-IL2CppDumper是刷机工具Magisk中的一个插件，相比于IL2CPPDumper的静态分析，Zygisk-IL2CppDumper有以下几个特点：**  

Zygisk-IL2CppDumper可以做到在游戏运行过程中，动态dump函数名和函数偏移，可绕过保护、加密以及混淆。  

启动游戏后，会在目录下自动生成dump.cs，即使游戏有检测到异常闪退，也不影响dump.cs的获取。  

Zygisk-IL2CppDumper采用github Action，仅需fork项目，填写包名等简单步骤就可逆向分析代码，极大程度降低了外挂、破解门槛。  

针对 Zygisk-IL2CppDumper 逆向分析工具对游戏造成的严重危害，FairGuard游戏加固依托十余年游戏安全对抗经验，研发了一套成熟完善的对抗方案，可有效提高分析破解门槛。目前，该方案已接入多家大厂旗下热门游戏，可行性与有效性得到了充分认证。  

**丨结语**  

FairGuard作为专注于游戏安全领域的第三方服务商，致力于帮助游戏公司解决外挂和破解问题，为游戏提供深度一体化的加密保护方案。  

目前产品已经被FunPlus、三七互娱、恺英网络、心动、杭州电魂等多家头部公司采用，接入400+款热门游戏。欢迎访问 [www.fair-guard.com](https://www.fair-guard.com) 了解试用。    

产品咨询联系方式：  

QQ：2079128588  

微信：fairguard01  

技术交流QQ群：1105310296  
