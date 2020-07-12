---
layout: post
title:  "Unity(iOS)AssetBundle资源内存动态分块加密方案"
date:   2020-07-12 13:30:13 +0800
categories: unity3d
tags: ab加密 assetbundle资源加密 ab资源加密 assetbundle资源加密 unity资源加密
---
##### **一、背景**

​      AssetBunlde资源包是unity引擎游戏对资源(声音、图片、脚本等)的压缩包，那我们为啥要保护AssetBunlde(以下简称AB包)？对于一般技术人员都清楚, 游戏逆向者(游戏外挂制作者)一般通过游戏资源修改工具(AssetStudio、UABE)修改之后重打包实现某种程度正常游戏达不到的功能，例如射击类修改资源后可以透视，等等，下面是某作者技术文章就说的很明白怎么破解AB包。

<img src = "/assets/res/202007/31.png" style="zoom:50%" />

<img src = "/assets/res/202007/32.png" style="zoom:28%" />

​    那市面上iOS端有没有比较好的加解密方案呢？通过调研，发现有些是Unity源码级别的，但是:  

1. 这对开发者的技术要求比较高  

2. 这种加解密只能做到整体加解密(会在内存中某个时间断暴露解密后资源)，所以说目前还没发现针对iOS端的“AB包内存动态分块加解密技术”

在此背景下，fariguard研发出了针对iOS端AB包资源独家内存动态分块加解密方案(此方案在不影响游戏的运行效率下极大程度保护了AB资源安全，且支持在线更新)

  
##### **二、**iOS侧技术难度

​     有些同学就会问，为啥iOS端相比安卓端就难实现呢？有如下几点原因：

  1、 iOS整个系统是闭源状态，APP运行时权限限制严格，代码段只能执行、数据段只能读取

  2、 iOS APP唯一上架渠APP Store，所有APP上架到APP Store之前都会受到苹果的严格审核。

​     那么FairGuard通过独家静态HOOK技术，通过解析AssetBundle文件格式进行Patch关键位置实现了内存动态分块加解密方案，这样就避开了由于苹果闭源导致App运行时权限不足以及上架问题。


##### **三、具体实现方案**

- 3.1 内存动态加解密方案

- 3.2.1、 概述-寻找最佳Patch函数

<img src = "/assets/res/202007/33.png" style="zoom:40%" />

通过寻找最终定位在libiPhone-lib.a里面的ArchiveStorage类，所有的上层加载AB资源的接口都会调用这个函数并把ab资源加载到内存。

- 3.2.2、 实现思维图导图如下  

<img src = "/assets/res/202007/34.png" style="zoom:50%" />

- 3.2.3、 方案优点

<img src = "/assets/res/202007/35.png" style="zoom:50%" />

##### **四、加密前后对比**

- 4.1、 加密前后AssetBundle资源效果

<img src = "/assets/res/202007/36.gif" style="zoom:60%" />
   
- 4.2、 加密后APP启动效果

<img src = "/assets/res/202007/37.gif" />

##### **五、写在最后**

​        欢迎大家转载并试用

<img src = "/assets/res/202007/38.png" style="zoom:50%" />

​    FairGuard建立之初即定位为技术驱动型公司，深耕技术，致力于打造业界顶尖的游戏加固产品。公司开发团队都是来自于网易易盾的核心人员，创始人专注于安全领域10多年，前网易易盾手游保护负责人，从0到1主导了易盾手游保护项目。欢迎访问 [www.fair-guard.com](https://www.fair-guard.com) 了解更多内容。