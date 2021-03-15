---
layout: post
title:  "Unity Resources 加密"
date:   2021-03-15 10:30:13  +0800
categories: unity加密
tags:  unity加密 Resources加密
---

Unity Resources类型的资源因为简单易用的特点，使其非常适合用于快速开发原型。目前还有很多游戏使用这种类型的资源，而不是使用Assetbundle。

Unity Resources资源文件打包后,在assets/bin/Data 路径下以hash值命名的文件形式存在，如下图所示：

![315_1](/assets/res/202103/315_1.png)

鉴于有多个游戏向FairGuard提出对这类资源进行加密的需求。

FairGuard对Unity Resources资源加载原理进行了深入分析，找到加解密的核心点，并构造了对资源进行随机加密的方案和算法。

原始的Resources文件，二进制如下图所示,可以看到很多的0以及资源版本等信息：

![315_2](/assets/res/202103/315_2.png)

下图是加密后的资源，已经看不到任何信息，而且每次加密后的文件内容都完全不同。

![315_3](/assets/res/202103/315_3.png)

再使用AssetStudio对资源进行解析，对比加密前后的效果。

先看一个未加密的资源。使用AssetStudio Load file加载该资源，加载后解析结果如下2图所示:

![315_4](/assets/res/202103/315_4.png)

![315_5](/assets/res/202103/315_5.png)

可以看到，AssetStudio解析出了很多信息。 

下图是使用AssetStudio加载加密后的Resources资源文件的结果：

![315_6](/assets/res/202103/315_6.png)

 AssetStudio无法识别。

此Resources随机化加密，支持安卓/iOS/Windows三平台。

结合之前已经实现的[*Assetbundle加密*](https://www.fair-guard.com/index/news-view.html?id=375)、global metadata加密、[*so加壳*](https://www.fair-guard.com/index/news-view.html?id=373)。FairGuard可对Unity游戏包体内游戏相关的几乎所有文件做随机化加密处理。

 

FairGuard专注于游戏加固与[***反外挂\***](https://www.fair-guard.com/index/pro.html?id=361)，对Unity引擎有深入研究，为Unity应用提供深度一体化加密保护方案。