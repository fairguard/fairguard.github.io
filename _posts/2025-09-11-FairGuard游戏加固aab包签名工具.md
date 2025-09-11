---
layout: post
title: "FairGuard游戏加固aab包签名工具"
date: 2025-09-11 16:30:00 +0800
categories: 游戏安全
tags: aab包签名  Google play  游戏上架
---

由于谷歌签名工具apksigner.jar，仅支持apk包签名，不支持abb包签名。为减少开发者额外适配成本，FairGuard在游戏加固工具中提供了aab签名相关功能。如有需要可以下载并使用FairGuard加固工具，对aab包体进行签名。<!-- more -->  

**访问路径**

FairGuard官网 → 帮助中心 → Android → 加固工具下载 → 下载游戏加固工具

![315_21](/assets/res/202103/下载.png)  
FairGuard游戏加固接入流程

点击[下载](https://www.fair-guard.com/help/list-228.html) 直达工具下载页面。

工具使用说明

该加固工具为 jar 包文件，适用于 Windows、Linux 及 Mac 系统，需安装 java 环境。如在 Mac 或 Linux 系统下使用，则需把 modules 目录下的文件赋予可执行权限。

命令行参数

java -jar FairGuard.jar -optype_sign_jar -inputfile %inputfile.aab% [-outputfile %output.aab%]

参数说明

-optype_sign_jar

必填项，表示进行AAB签名操作

-inputfile

必填项，参数后面跟待签名的AAB文件路径

-outputfile

可选项，输出文件路径名。如未设置此选项，则输出文件路径默认为% inputfile.aab %同目录，命名为以%inputfile_signed.aab %为结尾的文件名

签名信息在 config.ini 文件中配置，配置如图所示：

![315_21](/assets/res/202103/config.png)  
FairGuard游戏加固接入流程

keystore-path: 签名文件路径，支持相对路径与绝对路径(路径不可包含中文)

alias: 签名别名

password: 对应签名信息的密码

alias-pwd: 对应签名别名的密码

以上4个信息，任意一项有误的话，签名均会失败。

如果只是做签名操作，gamekey不会做验证，可随意设置。

AAB签名命令行示例:

java -jar FairGuardx.x.x.jar -optype_sign_jar -inputfile game.aab

![315_21](/assets/res/202103/演示.png)  
AAB签名效果演示

完成签名后的 aab 包可正常提交Google Play 上架。

作为专业的应用安全解决方案，FairGuard在提供签名功能的同时，还搭建了针对不同场景及游戏类型的功能矩阵，服务方案涵盖游戏加固、防破解、反外挂、通讯保护、环境检测、经济安全、云保护、态势感知等多项内容。

**丨结语**  

FairGuard作为专注于游戏安全领域的第三方服务商，致力于帮助游戏公司解决外挂和破解问题，为游戏提供深度一体化的加密保护方案。  

目前产品已经被FunPlus、三七互娱、恺英网络、心动、杭州电魂等多家头部公司采用，接入600+款热门游戏。欢迎访问 [www.fair-guard.com](https://www.fair-guard.com) 了解试用。    

产品咨询联系方式：  

QQ：2079128588  

微信：fairguard01  

技术交流QQ群：1105310296  