---
layout: post
title:  "iOS 代码加密方案"
date:   2020-07-12 15:30:13 +0800
categories: iOS
tags: iOSApp防护 iOSApp加固保护 iOS代码加密
---
##### 一、前言
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;iOS 系统从 iOS 1 (2007) 到目前 iOS 13 (2020)，已经经历了 13 代，和安卓的开源生态相比 iOS 一直是都是闭源生态，这确实相对安全些，但是这也容易被人们忽略了其自身 App 的安全性，导致大部分公司开发 App 时不会投入成本在自身 App 的安全方面，一旦系统越狱后被逆向研究 App，结果往往是比安卓更恐怖的，所以说在系统闭源前提下，我们也需要对 App 自身的安全进行加固防护，不能太依赖系统的环境。特此 FairGuard 提供一套完整的 iOS 代码加密方案，一键傻瓜式操作。<br/><!-- more --><br/>

##### 二、 iOS App 保护

iOS App 客户端的加密防护工作可以分为如下几个角度：

 **1、代码本身防护**

​      &nbsp;&nbsp;&nbsp;&nbsp;1.1  代码逻辑的混淆

​      &nbsp;&nbsp;&nbsp;&nbsp;1.2  字符串的加密

​      &nbsp;&nbsp;&nbsp;&nbsp;1.3  Objective-C 类名和方法名的混淆

 **2、代码行为防护**

​      &nbsp;&nbsp;&nbsp;&nbsp;2.1  本地存储时的保护 (NSUserDefaults，sqlite 文件数据加密)

​      &nbsp;&nbsp;&nbsp;&nbsp;2.2  网络传输数据保护

 **3、代码对抗防护**

​      &nbsp;&nbsp;&nbsp;&nbsp;3.1  反调试保护

​      &nbsp;&nbsp;&nbsp;&nbsp;3.2  反越狱保护

​      &nbsp;&nbsp;&nbsp;&nbsp;3.3  反重签名保护

​      &nbsp;&nbsp;&nbsp;&nbsp;3.4  反修改器保护
<br/><br/>
##### 三、 代码本身防护  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;目前能用的方案大部分是基于 IR( LLVM 编译框架的一个中间代码)层做混淆 Pass，这也是针对代码逻辑混淆的比较好的方案。目前网上有一个项目是 Obfuscator-LLVM，由[瑞士伊夫尔东莱](http://www.heig-vd.ch/international)班的应用科学与艺术大学信息安全小组研发的，完全开源，接入的话是编译成 XcodeToolchain 加入混淆编译 C/C++ Flag,支持 控制流平坦化、伪代码插入, 但是不支持字符串混淆这个有另外一个项目。

下面我们可看看使用 FairGuard iOS 加固方案混淆后的效果图:  

<img src = "/assets/res/202007/51.png" style="zoom:30%" />  

<img src = "/assets/res/202007/52.png" style="zoom:35%" />  

<img src = "/assets/res/202007/53.png" style="zoom:30%" />  


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;我们 FairGuard 目前支持代码逻辑混淆、字符串混淆，也是基于 LLVM 的 IR 层的混淆 Pass，但是不是使用开源的 LLVM 项目，而是通过 HOOK 了 Xcode 里面 Apple 的 clang，在其执行优化的过程拦截执行我们优化 Pass，最小程度的减少用户接入成本和体验效果。对用户来说完全无感知。<br/><br/>

##### 四、代码行为防护

  **2.1  本地存储时的保护(NSUserDefaults，sqlite 文件数据加密)**

​    在执行 NSUserDefaults、sqlite 存储操作的时候对文件进行加密处理在存储即可如：

> // 获取需要加密文件的二进制数据  
> NSData *data = [NSData dataWithContentsOfFile:@“~/Desktop/fairGuard.jpg"];   
> // 对 Data 进行加密  
> NSData *deData = [data decryptDate];   
> // 最后将加密后的文件存储到到 NSUserDefaults 或者 sqlite  

  **2.2  网络传输数据保护**

​      对客户端传输数据提供加密方案，有效防止通过网络接口的拦截获取数据,。

​     Md5(完整性): 传输前对整个数据做一个 Md5 保证数据的完整性。

<img src = "/assets/res/202007/54.png" style="zoom:40%" />

​      AES(机密性): 传输前对 Body 数据做一个加密保证数据的机密性。

<img src = "/assets/res/202007/55.png" style="zoom:35%" />
<br/><br/>
##### 五、代码对抗防护

当你的代码本身和行为都做了很好的防护后，逆向者可能会进行动态调试，这时候就需要代码运行时的对抗防护了。

**3.1 反调试保护**

&nbsp;&nbsp;&nbsp;&nbsp;可以使用网上公开的 ptrace、syscall、sysctl、异常等方式, 之后再对这段防护代码加上之前的保护后效果会更佳。

**3.2 反越狱保护**

&nbsp;&nbsp;&nbsp;&nbsp;可以从多个维度检测, 例如检查某些 App 的是否安装、某些文件是否存在、某些目录的访问权限来综合判断。

**3.3 反重签名保护**

&nbsp;&nbsp;&nbsp;&nbsp;原理可以用加固时存储当时的 BundleId 和当时二进制的签名，运行时也获取一遍并且与加固时的进行对比来防护。

**3.4 反修改器保护**  <br/><br/>


在代码对抗防护方面，**FairGuard 不但提供本地检测代码，并且保护了检测代码，还提供了在线特征更新检测，定期更新，在最短的时间保护了 App 的安全性**。如下：

<img src = "/assets/res/202007/56.png" style="zoom:35%" />
<br><br/>
##### 六、FairGuard 一键加固演示
<img src = "/assets/res/202007/57.gif" />
<br/><br/>
##### 七、写在最后

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FairGuard 建立之初即定位为技术驱动型公司，深耕技术，致力于打造业界顶尖的游戏加固产品。公司开发团队都是来自于网易易盾的核心人员，创始人专注于安全领域 10 多年，前网易易盾手游保护负责人，从 0 到 1 主导了易盾手游保护项目。欢迎访问 [www.fair-guard.com](https://www.fair-guard.com) 了解更多内容。
