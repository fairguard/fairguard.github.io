---
layout: post
title:  "App bundle(aab包)如何签名并生成签名工具"
date:   2021-03-15 11:10:10  +0800
categories: Android签名
tags:  aab包 Appbundle 如何签名 签名工具
---

如果对App bundle包内的文件做了修改，或者增删文件，就需要重新签名才能在google play提交上架，不然会出现如下图的报错。

![image001.png](/assets/res/202103/315_10.png)

网上搜了一圈，并没有找到可对aab包签名的工具。

apk包签名google提供了现成的工具apksigner.jar，这个jar包并不能对aab包签名。如果把aab包当成apk，用这个工具来签名，会出现如下图的报错：

![315_11](/assets/res/202103/315_11.png)

这个apksigner.jar应该是会去识别apk里面的目录结构，非apk结构的包不能做签名。

尝试使用jarsigner的签名方式，使用方法如下：

jarsigner -digestalg SHA1 -sigalg MD5withRSA -keystore android.keystore -storepass android -signedjar enhanced_sign.apk enhanced.apk android.keystore

使用上面的命令行对apk签名是正常的，也可以安装。

使用该命令行对aab包进行签名，签名提示成功了，但是将签名后的包提交到google play，会有如下的报错：

![image004.png](/assets/res/202103/315_12.png)

看来此方法对aab包的签名，google play不认。

尝试使用不同的签名参数进行签名，然后对签名后的mf文件进行比对，最后终于成功找到可以让google play验证通过的aab包签名方式。

将该方法封装成了一个签名工具，可访问[**此页面**](https://www.fair-guard.com/index/helpcenter.html?id=387)下载。

使用方法，先把签名信息在config.ini文件里做类似如下的设置：

  ![image005.png](/assets/res/202103/315_13.png)

然后运行如下命令行：

java -jar FairGuard2.0.4.jar -optype_sign_jar -inputfile %inputfile.aab%

命令行执行后如下图界面所示：

![image006.png](/assets/res/202103/315_14.png)

使用此工具签名后的aab包即可正常提交到google play.

&nbsp;

&nbsp;

FairGuard 专注于游戏加固与[反外挂](https://www.fair-guard.com/index/pro.html?id=361)，致力于打造业界顶尖的游戏保护方案。

研发了 SO 无导入函数加壳、iOS AB 资源加密、虚拟机底层指纹检测等多项独家保护功能。目前已被 FunPlus、三七互娱、游族等多家头部游戏公司采用。