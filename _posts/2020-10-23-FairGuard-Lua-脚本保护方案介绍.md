---
layout: post
title:  "FairGuard Lua 脚本加密方案介绍"
date:   2020-10-23 18:30:13  +0800
categories: 防破解 lua
tags:  lua加密 脚本保护
---

本文从 Lua 使用场景 / Lua 脚本安全问题 / 常见的 Lua 保护方案 等几部分，介绍我们的 Lua 脚本保护方案。<!-- more -->

#### 一. Lua 在手游中的使用场景

​		Lua 作为一种小巧而灵活的脚本语言被越来越多的手游所使用。

**1. Cocos2dx 引擎**

   在 Cocos2dx 引擎中，可选的脚本语言主要有 Lua 与 Javascript。相对于 Javascript, 因 Lua 更适合制作非 h5 游戏而被广泛使用。

   ```shell 
   ~/> cocos new --help
   usage: cocos new [-h] [-p PACKAGE_NAME] [-d DIRECTORY] [-t TEMPLATE_NAME]
                [--ios-bundleid IOS_BUNDLEID] [--mac-bundleid MAC_BUNDLEID]
                [-e ENGINE_PATH] [--portrait] -l {cpp,lua,js}
                [PROJECT_NAME]
   ```

**2. Unity3d 引擎**

​		Unity3d 引擎的原生脚本语言是 C#, 但由于 iOS 系统安全限制无法热更新 C#, 从而出现了许多使用 Lua 的热更新框架，如 toLua / uLua / xLua 等。这些框架将 Unity3d 引擎 API 封装为 Lua 接口，让游戏开发人员拥有使用 Lua 脚本开发游戏逻辑/界面的能力。需要热更新时，服务端可以动态下发 Lua 脚本，客户端加载新的 Lua 脚本即可更新游戏逻辑/界面等。

#### 二. Lua 脚本安全问题
​		由于 Lua 是一种解释型语言，所以 Lua 虚拟机可以直接解释执行 Lua 源代码，这就导致许多游戏开发者直接将 Lua 源代码打进 apk / ipa 中，这相当于直接泄漏了游戏源代码，大大降低了外挂制作门槛，更有可能被拿去做换皮肤二次开发。在 Cocos2dx 中，若直接使用 cocos compile 编译打包（不加 --compile-script 参数），那么 Lua 脚本就是以源代码形式打包的。

​		在实际开发中，开发人员可以选择使用官方 Luac 或者 LuaJIT 将 Lua 脚本编译为字节码，但它们也都面临相似的安全问题。

**1. 官方 Lua**

   开发人员可以使用 Luac 将 Lua 源代码编译为 Lua 字节码。

   ```lua 
   ~/ > cat test.lua
   print("hello world")
   ~/ > ./luac -s -o test_lua.bytes test.lua
   ~/ > hexdump -C test_lua.bytes
   0000  1b 4c 75 61 53 00 19 93  0d 0a 1a 0a 04 08 04 08 |.LuaS...........|
   0010  08 78 56 00 00 00 00 00  00 00 00 00 00 00 28 77 |.xV...........(w|
   ........
   0040  80 00 02 00 00 00 04 06  70 72 69 6e 74 04 0c 68 |........print..h|
   0050  65 6c 6c 6f 20 77 6f 72  6c 64 01 00 00 00 01 00 |ello world......|
   ```

   从上面的字节码看，已经不包含源代码，只包含函数名称及字符串。不过 Lua 字节码非常容易被 luadec / unluac 等工具反编译，而且反编译得到的 Lua 代码几乎与源代码一致。

**2. LuaJIT**

   由于 LuaJIT 在开启 JIT 的情况下可以数十倍提升 Lua 代码执行效率[2]，所以非常多游戏/框架都采用 LuaJIT 替代官方 Lua 虚拟机。

   ![luajit_performance](/assets/res/202010/luajit_performance.png)

   对应地，开发人员可以使用 luajit 可执行程序将 Lua 源代码编译为 luajit 的字节码。

   ``` lua
    ~/ > luajit -b test.lua test_jit.bytes
    ~/ > hexdump -C test_jit.bytes
   0000  1b 4c 4a 01 01 29 02 00  02 00 02 00 04 36 00 00  |.LJ..).......6..|
   0010  00 27 01 01 00 42 00 02  01 4b 00 01 00 10 68 65  |.'...B...K....he|
   0020  6c 6c 6f 20 77 6f 72 6c  64 0a 70 72 69 6e 74 00  |llo world.print.|
   ```

   luajit 的字节码也有相应的反编译器，比如 ljd [3] [4], luajit-decomp[5]。

#### 三. 常见的 Lua 保护方案
##### 1. Cocos2dx xxtea 加密

​		cocos compile 命令提供了 --lua-encrypt 等参数用于加密 Lua 脚本：

	luac 相关参数:
	--lua-encrypt     开启 XXTEA 加密功能。
	--lua-encrypt-key LUA_ENCRYPT_KEY       指定 XXTEA 加密功能的 key 字段。
	--lua-encrypt-sign LUA_ENCRYPT_SIGN    指定 XXTEA 加密功能的 sign 字段。


​		但是这个加密非常容易被破解，外挂制作者可以很容易就在 libcocos2dlua.so 中找到 xxtea 的密钥, 之后就可以解密出脚本  


##### 2. 自定义 Lua 操作码

​		有些游戏会自定义 Lua 操作码，使得常规的 luadec / unluac 无法反编译字节码。某大型回合制游戏就是采用了这种保护方法，它可以提高一些外挂制作门槛，但由于代码自身保护强度不够，被轻易还原操作码，进而被反编译[1]。

![opcode](/assets/res/202010/opcode.png)

#### 四. FairGuard 推出的 Lua 保护方案

​		FairGuard 针对 Lua 脚本可能存在的各方面安全问题，在深入理解 Lua 语言特性与虚拟机底层的基础上，推出全新的 Lua 脚本保护方案，从各个方面确保 Lua 脚本不被分析破解。

**1. 字节码文件深度加密**

​		Cocos2dx 使用 xxtea 整体加密脚本文件，这种方式不仅易于被分析出加密密钥，也容易被 hook luaL_loadbuffer 而得到明文文件。我们采用深度加密的方式，确保 Lua 脚本无法被直接解密，确保无法通过 hook luaL_loadbuffer / lua_reader 等函数截获明文脚本文件。

​		正常的字节码：

```lua
~/ > hexdump -C test_lua.bytes
00000000  1b 4c 75 61 53 00 19 93  0d 0a 1a 0a 04 08 04 08  |.LuaS...........|
00000010  08 78 56 00 00 00 00 00  00 00 00 00 00 00 28 77  |.xV...........(w|
00000020  40 01 00 00 00 00 00 00  00 00 00 00 01 02 04 00  |@...............|
00000030  00 00 06 00 40 00 41 40  00 00 24 40 00 01 26 00  |....@.A@..$@..&.|
00000040  80 00 02 00 00 00 04 06  70 72 69 6e 74 04 0c 68  |........print..h|
00000050  65 6c 6c 6f 20 77 6f 72  6c 64 01 00 00 00 01 00  |ello world......|
```

​		我们保护后的字节码：

```lua
~/ > hexdump -C test_protected.lua
00000000  1b 1b 1b 03 04 84 a6 d6  b2 48 d2 2e 35 5d ee 68  |.........H..5].h|
00000010  e3 88 78 2b 30 05 b3 11  21 44 78 ae 01 06 db 8f  |..x+0...!Dx.....|
00000020  16 26 da 8e 16 db ed 45  0a 2d fb d0 42 3e 28 4d  |.&.....E.-..B>(M|
00000030  48 12 fa df a2 9e 02 1a  9f 3c 2e 8b b8 8c d5 00  |H........<......|
```

**2. 运行时动态保护**

   FairGaurd 深入虚拟机引擎，使用压缩/混淆/动态变形等方式定制了独有的运行时信息，进一步提高逆向分析难度。

**3. Lua 虚拟机代码保护**

   配合高强度的 so 加固，使得 Lua 虚拟机代码不易被逆向分析。

   正常的 Lua 虚拟机 Native 层代码，可以看到非常多的 Lua 接口：

   ![ori_lua_so](/assets/res/202103/lua1.png)

   我们处理后的 Lua 虚拟机 Native 层代码，看不到没有任何接口：

   ![protected_lua_so](/assets/res/202103/lua2.png)

**4. 接入方便**

   接入时无需更改上层 Lua 脚本代码，对开发者透明。



#### 五. 小结 

​		FairGuard 从多个攻击角度审视 Lua 脚本安全问题，从 Lua 语言多各个层面出发，推出了全新的 Lua 脚本保护方案，该方案能为 Lua 代码提供高强度的保护。同时支持官方 Lua 与 LuaJIT，支持 Cocos2dx 引擎，支持 uLua / toLua / xLua 等 Unity3d 插件。

​		欢迎访问我们的官方网站 [https://www.fair-guard.com](https://www.fair-guard.com)  , 了解更多信息。



#### 参考
1. https://bbs.pediy.com/thread-216969.htm
2. https://luajit.org/performance_arm.html
3. https://github.com/DrNewbie/luajit-decompiler
4. https://github.com/NightNord/ljd
5. https://github.com/bobsayshilol/luajit-decomp
6. https://www.52pojie.cn/thread-594286-1-1.html
