---
layout: post
title:  "FairGuard 打击外挂,守护游戏安全"
date:   2020-08-06 22:30:13  +0800
categories: 反外挂
tags: 反外挂 防破解 游戏外挂
---

据 Sensor Tower 商店情报数据，2019 年全球移动游戏仅在 App Store 和 Google Play 的总收入就达到 617 亿美元，较 2018 年增长14.8%, 中国手游市场在 2019 年依然处于上升通道。在巨大的利益驱使下, 手游黑灰产已经形成并且来势汹汹, 对游戏公司造成了难以估量的经济损失。<!-- more -->

FairGuard 正是针对这一场景而创建的游戏安全加固产品。公司成员都是来自前网易游戏加固团队的核心骨干。专注于游戏加固及反外挂，致力于打造业界顶尖的游戏加固产品。

FairGuard支持 Android/iOS/PC 多个平台,适应于Unity/Cocos/UE 等多种游戏引擎。游戏上线前, 我们加固游戏防止破解; 游戏上线后, 我们监控新外挂/外挂变种, 动态下发特征打击外挂。

 

#### 一. 外挂带来的损失

**1. 手游破解版/外挂泛滥, 严重破坏游戏平衡, 导致玩家迅速流失, 大幅减少游戏收益。**

比如对于 FPS 游戏, 外挂肆意修改内存, 可以实现许多匪夷所思的功能, 飞天、遁地、透视、爆头, 无所不能。外挂玩家毫不意外将秒杀正常玩家, 大大打击正常玩家的积极性, 最终导致玩家流失。另一方面, 被外挂行为蛊惑的正常玩家也可能购买使用外挂, 进一步破坏游戏环境, 使游戏环境陷入恶化循环。

![image001.png](/assets/res/202010/1.png)

比如对于 SLG/RPG 游戏, 外挂可以利用模拟点击实现全天 24 小时不休息刷副本, 最后将得到的装备/角色出售获利。这种外挂行为在造成游戏服务器高负载、增加公司 IT 成本之外, 还可能引起游戏装备系统/经济系统通胀, 如不及时遏制, 最终可能导致游戏经济系统崩溃。

![image003.png](/assets/res/202010/2.png)

 

**2. 破解/外挂行为增加公司运行成本, 影响公司/品牌形象。**

游戏公司将花费大量的人力和技术手段监控游戏中的外挂行为, 花费大量的客服资源处理来自正常玩家的投诉。  

**3. 游戏代码/美术资源被竞争对手反编译后抄袭/盗用, 快速复制并上线运营, 对游戏公司造成难以估量的损失。**

手游公司在研发前期为了快速出游戏原型, 很可能从市面上已有的类似游戏中获取美术资源。比如一款 unity3d 游戏, 如果它的资源没有加密过, 通过 AssetStudio 等工具分分钟就可以提取出原始美术素材。

![image005.png](/assets/res/202010/3.png)

<<光环 4>>在去年就发生过美术素材被抄袭事件。

####  二. FairGuard 守护游戏安全

公司开发团队都是前网易安全部门核心人员,创始人为前网易高级技术专家,有10年以上的游戏安全从业经历, 从 0 到 1 主导了网易手游保护项目,为网易内外部数百个游戏提供了加固服务。我们重新出发,只为给用户提供更优质的游戏安全服务。

我们的游戏加固产品 FairGuard,经过与用户的适配打磨,经过数十个版本迭代, 经过与线上数百款外挂对抗,已经达到国内外反外挂顶尖水平。下面简单介绍下 FairGuard 的功能与优势。 

**1.　深入操作系统底层**

在一开始, 我们就定位我们的对手是国内外最顶尖的外挂制作者,他们具有深厚的逆向分析能力。而越底层的接口代码，越不容易被 hook 篡改; 混淆、加密强度越高的代码，越难被逆向分析。所以我们深入研究操作系统底层, 从系统底层 api 出发, 甚至手写汇编, 实现最基础的功能, 再配合私有的代码混淆技术/虚拟化技术, 以保证这些基础功能的可靠性，甚至我们研发了国内独有的 so 无导入函数加密来保护代码安全。FairGuard 从底层避免被逆向分析的问题。如下图所示，我们的基础功能代码难以被逆向分析：

![image007.png](/assets/res/202103/无导入函数代码流程图.png)

市面上有些同类产品, 正是因为基础功能的不可靠, 才导致溃于蚁穴的事经常发生。

**2.　深入研究游戏引擎**

我们深入研究了各种常见的游戏引擎，最终实现了与引擎底层紧密耦合的加固功能。与引擎底层绑定有几个优点：

1）可以实现更强大的防护功能。比如我们实现了国内独有的支持 Android/iOS 双平台的 unity3d assetbundle 资源加密功能；比如我们为 mono dll 实现了高强度的结构虚拟化。如下所示，虚拟化后的 dll 无法被 dnspy 分析：

![image009.png](/assets/res/202010/5.png)

2）不易被破解者摘除保护功能。正是因为与引擎底层耦合，如果破解者尝试摘除我们的保护功能，那么引擎代码也会被破坏，导致游戏无法运行。

**3.　防破解全面**

我们反复研究了市面上不同类型游戏遇到的各种破解问题, 提炼出破解方向进行防御。于是, 我们在可靠的基础功能上实现了一系列防破解功能。

常见的防破解功能有：

> 防盗版

签名和文件完整性多重校验，防止游戏被植入恶意模块、剔除广告等

> 防内购破解

防止篡改支付过程绕过付费或者替换收款帐号掠夺玩家付费 

>  反调试

防止外挂作者对进行调试，阻止对游戏的静态或动态分析

> 防内存篡改

防止通过篡改游戏内存，改变游戏属性的作弊行为，防止修改器的使用

>  防地理位置篡改

禁用地理位置模拟操作，防止LBS类型玩法的作弊

>  反ROOT反越狱

禁用ROOT(越狱)手机，或对ROOT(越狱)进行检测

>  加密游戏资源

加密游戏美术资源、数值配置文件，防止被盗用、抄袭

> …...

**4.　反外挂智能**

我们提供的传统反外挂功能有：

> 反修改器

独创技术精准识别各种修改器，包含GG修改器、八门神器等及其变种

> 反变速器

独创的无导入函数引擎加固，使得在游戏中使用任何变速器、加速器不产生效果

> 反虚拟机

独创技术精准识别VirtualAPP、平行空间、VirtualXposed等虚拟机或多开器

> 反云手机

精准识别红手指、蓝光云、河马云等云手机，打击影响游戏平衡的挂机

> 反挂机挂

禁止玩家使用按键精灵、蜂窝助手等模拟点击脚本、挂机挂等外挂 

> 反注入器

禁止使用Xposed、Frida等各种外挂模块注入器，防止注入后修改游戏内存等各种恶意行为

> 反模拟器

采用底层技术精准识别市面所有模拟器，打击作弊、自动化挂机等行为

> …...

 

在传统反外挂功能之外，还可以通过后台大数据分析自动发现异常，我们再提炼外挂行为、特征动态下发，有效打击新型外挂、外挂变种。

![image011.png](/assets/res/202010/7.png)

 

####  三. 游戏保护效果展示

自 FairGuard 运营以来, 我们解决了用户线上遇到的各种各样的被破解/外挂问题。这里向大家分享几个具有代表性的用户案例。 

**1.  某主要营收来源于广告的休闲类游戏**

**问题**:

在接入 FairGuard 前, 网络上出现了很多去除广告破解版, 甚至有些破解版本更改了广告 id。

**解决方案**:

1) 对游戏包加固, 防止破解者改包

2) 对 unity3d dll 做高强度虚拟化, 防止被篡改游戏逻辑

**效果**: 该游戏接入 FairGuard 加固上线后至今，**各个破解版平台未再出现破解版本，游戏官方下载量增长30%**。 



**2.  某百万级用户量的 Unity3d 引擎二次元游戏**

**问题**:

1) 游戏脚本未做加密，被破解修改后重打包，破解版提供无敌、秒杀、过图等功能。

2) 游戏通讯协议简单，协议被破解，出现使用挂 VPN 方式的伪脱机挂。

3) 存在很多的工作室盈利行为，对游戏营收造成严重影响。

**解决方案**:

1) 对游戏的 Unity 脚本做最高强度的结构虚拟化加密，防止游戏逻辑被篡改。

2) 此类游戏修改内存会实现变态的功能，为此配置了反内存修改、反加速、反虚拟机功能，防止玩家使用此类外挂。

3) 针对通讯协议被破解，我们提供数据签名SDK让游戏接入，通过安全强度极高的数据校验算法，来防止脱机伪造通讯包。

4) 通过智能化的威胁数据分析，定位到多个工作室和专业破解者。

**效果**: 该游戏接入 FairGuard 加固上线后，游戏方运营反馈，**客户外挂投诉量立马减少95%**。 



**3.  某百万级用户量的 cocos单机游戏**

**问题**:

1) 游戏被破解解密出脚本，去除游戏方自带的签名校验。

2) 游戏脚本被修改后，把充值接口和界面进行替换，做成私服，私服方做成道具收费自己运营。

3) 除了私服外，还有很多修改器、加速器等外挂，对游戏营收造成严重影响。

**解决方案**:

1) 对游戏引擎做定制化混淆和加固，防止脚本被解密。

2) 增加防修改器、防加速器、防破解重打包功能。

3) 通过云特征来做线上动态防御。

**效果**: 本来游戏更新后，私服五天即会更新对应的私服版本，该游戏接入 FairGuard 加固上线后，至今已经2个多月都未更新版本。游戏营收相比未接入前增长30%。

 

####  四. 结语

FairGuard 将在与游戏外挂对抗中不断升级, 持续为用户提供优质的防破解/反外挂服务,守护游戏安全。

欢迎访问 [www.fair-guard.com](http://www.fair-guard.com/) 获取更多信息

联系电话：17156492281

