---
layout: post
title:  "Unity il2cpp global-metadata.dat 加密方案"
date:   2021-03-25 17:30:13  +0800
categories: unity加密
tags:  unity加密  global-metadata.dat加密 il2cpp加密
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;众所周知，Unity 游戏有两种打包方式 mono 与 il2cpp。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在 mono 模式下，游戏 C# 代码被编译为 IL(中间代码) 并生成 dll 文件，然后 dll 被打包进最后的游戏包文件。由于 IL 非常容易被 ILSpy / .NET Reflector 等专业反编译软件分析逆向，所以在无保护情况下，游戏的安全性极差。如下图，.NET Reflector 几乎可以还原出 C# 文件，外挂开发者可以凭此迅速制作出强大的外挂：

![1](/assets/res/202103/325/1.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于 mono, il2cpp 将游戏 C# 代码转换为 C++ 代码，然后编译为各平台 Native 代码。由于 Native 代码的逆向/反编译难度大大增加，可以有效提高外挂开发/游戏破解的门槛。另外，il2cpp 具有执行效率高/平台兼容性好等原因，所以 il2cpp 已经逐步被绝大多数游戏采用。
    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在 il2cpp 模式下，虽然游戏逻辑是以 Native 代码运行, 但依然要实现 C# 某些语言特性（如GC), il2cpp 将所有的 C# 中的类名/属性名/字符串等信息记录在 global-metadata.dat 文件。il2cpp 启动时会从这个文件读取所需要的类名/属性名等信息。正是这一机制为外挂制作/游戏破解带来了便利，使用 IL2cppDumper 可以解析 global-metadata.dat 文件，并将文件里的类名等字符串信息对应到 Native 代码中去，极大地方便了逆向分析。IL2cppDumper 接收 libil2cpp.so / global-metadata.dat 两个文件路径为输入：

![2](/assets/res/202103/325/2.png)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基于这种情况，FairGuard 开发了 global-metadata.dat 加密方案（同时支持 Android / iOS / Windows），防止 global-metadata.dat 被 IL2cppDumper 解析。该方案主要包括两个部分：

1、加密 global-metadata.dat 文件，同时做到对开发者透明。开发者只需要上传游戏包文件，就可以实现加密，不需要上传额外文件。加密前 globa-metadata.dat 文件：

![3](/assets/res/202103/325/3.jpg)

加密后的 global-metadata.dat 文件：

![4](/assets/res/202103/325/4.jpg)

2、加密 libil2cpp.so 文件。由于 IL2cppDumper 需要依赖 libil2cpp.so 对应 global-metadata.dat 文件中的字符串地址，所以对 libil2cpp.so 做深度加密非常有必要。即使从内存中 dump 出来 libil2cpp.so , 依然不会被 IL2cppDumper 正常识别。

FairGuard 会使用独创的无导出/[**无导入函数SO加壳方案**](https://www.fair-guard.com/index/news-view.html?id=373)，对il2cpp进行加壳。效果如下所示，原始未加壳il2cpp.so有519个导出函数：
   
   ![image.png](/assets/res/202103/325/6.png)
   
   FairGuard加壳后il2cpp.so，无导出函数：
   
   ![image.png](/assets/res/202103/325/7.png)
   
   原始未加壳il2cpp.so有252个导入函数：
   
   ![image.png](/assets/res/202103/325/8.png)
   
   FairGuard加壳后il2cpp.so，无导入函数：
   
   ![image.png](/assets/res/202103/325/9.png)
   
使用这个方案加壳后，即使从内存中 dump 出来 libil2cpp.so , 依然不会被 IL2cppDumper 正常识别。效果如下：

![5](/assets/res/202103/325/5.png)



结语：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FairGuard专注于游戏加固与[***反外挂\***](https://www.fair-guard.com/index/pro.html?id=361)，对 Unity 引擎有深入研究，为 Unity 应用提供深度一体化加密保护方案。 目前产品已经被FunPlus、三七互娱、游族等多家头部公司采用。TapTap排行榜以及买量排行榜上亦有多个游戏使用。

有兴趣的加QQ号：*[2079128588](http://wpa.qq.com/msgrd?v=3&uin=2079128588&site=qq&menu=yes)* 或者 微信 fairguard001 咨询