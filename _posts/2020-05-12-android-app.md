---
layout: post
title:  我的Android必备应用
date:   2020-05-12 18:01:00 +0800
categories: document
tag: 安卓
music-id: 28365297
---

> 这几天翻出了我的小米5s与红米note4x，打算充分利用这些设备的余温，于是刷了一下机。不得不说现在刷机还是蛮简单的，一点都不麻烦。

<!--more-->
<!-- more -->

## 小米手机刷机

比想象中的还要简单：

1. **先解 bootloader 锁。** 这个还是挺简单的，用官方的软件解就行。
2. **输入 TWRP。** 个人感觉 TWRP 类似于 BIOS？总之就是用来刷系统的底层系统。网上已经有自动刷 TWRP 的脚本：[小米全机型刷入Recovery一键刷机教程](http://www.romleyuan.com/lec/read?id=201)
3. **双清。** 按音量上+电源键开机，然后按住音量上直到进入 TWRP，然后进入`清除`，滑动滑条即可完成双清。
4. **刷入rom。** 把 rom 拷入手机（在 TWRP 下依然可以连接电脑传数据），然后在安装那里选择rom，滑动滑条确定。

如果用的是国人自制的rom，那应该直接重启就能用。如果是国外贴近原生的rom，那可能会有点麻烦。你可能需要跳过初始设置，方法如下（[参考](https://www.v2ex.com/t/485478)）：

1. 在 TWRP 下，挂载 System
2. 然后在 `高级` 里进入命令行
3. 终端里 'vi system/build.prop' , 最前面加一行 'ro.setupwizard.mode=DISABLED' （[vi 教程](https://coolshell.cn/articles/5426.html)）

你可能会需要 root 权限，所以推荐一下 [magisk](https://github.com/topjohnwu/Magisk)。安装方法也很简单。去 Release 下载 zip 安装包（不是源码），然后无需双清，用上面第 4 步的方法刷入即可。

原生系统是通过访问 Google 来判断网络是否连接，所以会出现可以浏览网页却提示网络无法连接，并且无法下载。这个问题可以安装 [CaptiveMgr](https://www.coolapk.com/apk/tech.evlsoc.captivemgr) 来解决（需要root）。

最后不得不说，小米手机对刷机真的超级友好。

## 安卓常用软件（附下载地址）

### 系统自带软件的代替品

#### 计算器

[Google 计算器](https://www.coolapk.com/apk/com.google.android.calculator) 原生安卓的计算器，material风格。

[小米计算器](https://coolapk.com/apk/com.miui.calculator) 功能很全面，适合生活中的各种计算。

[GeoGebra 计算器](https://pan.baidu.com/s/1yYj_n29Y6hRb9KRWyHob8Q 
)（提取码：tsgc）科学计算器，忘带卡西欧时可以拿来凑合。

#### 浏览器

[VIA 浏览器](https://www.coolapk.com/apk/mark.via) 极致小巧，同时基本的去广告功能也有，还可以安装js插件：[via轻插件
](http://via-app.cn/#/tabBar/home)，主页也可以自定义。

[夸克](https://www.myquark.cn/) 其实也不错，视频下载功能特别适合下载小视频 (～￣▽￣)～。可惜加入了AI助手后就变得像一个搜索引擎了，体积也臃肿了很多。

#### 视频播放器

[VLC](https://get.videolan.org/vlc-android/3.2.11/) 全平台的开源播放器，还可以看局域网 DLNA、samba 的视频。

[MXplayer](https://www.90pan.com/wap.php?action=view&file_id=1584807) 和 VLC 同样强大的播放器，但不能看局域网的视频，而且普通版有广告，不过我已经给了无广告的 pro 版。

#### 输入法

[谷歌拼音输入法](https://www.lanzous.com/i3psqif) 简单，而且输入希腊字母很方便。可惜已经停更了，谷歌转去开发 [Gboard](https://apkpure.com/gboard-the-google-keyboard/com.google.android.inputmethod.latin)。

#### 图库

[快图浏览](https://www.coolapk.com/feed/18730943?shareKey=Yjg2NDU2YjRiMmJiNWVjNWZhN2Q~&shareFrom=com.coolapk.market_10.2.1) 自从作者不再更新后，WSTprojects 接手了，依然小巧好用。

[Simple Gallery](https://github.com/SimpleMobileTools/Simple-Gallery/releases) 快图浏览的替代品，不过体积大了好多。

#### 文件管理

[ES文件浏览器](https://coolapk.com/apk/com.estrongs.android.pop) 功能最全的文件浏览器，有广告也值了。

[ZArchiver](https://www.coolapk.com/feed/15826356?shareKey=NTkyZGZmYzNiYjI0NWVjNjBmNDU~&shareFrom=com.coolapk.market_10.2) 非常好用的解压工具，也可以当作文件管理。

#### 音乐播放器

我之前用的是 [Phonograph Music Player](https://coolapk.com/apk/com.kabouzeid.gramophone)，然而后来居然连定时播放都要付费才能用，于是就换到了 [Retro Music](https://coolapk.com/apk/code.name.monkey.retromusic)，现在在用国内开发者的 [APlayer](https://coolapk.com/apk/remix.myplayer)。

自从不能从网易云音乐下载 MP3 后，就很少用本地播放器了。

### 工具

#### Bmap白马地图

[Bmap白马地图](Bmap白马地图)，融合了百度和高德地图数据，无广告。

#### Fing WiFi扫描

[Fing](https://pan.baidu.com/s/1-v1wpjv8TXZT51V80a78GQ)（提取码：xvfb）可以看当前 WiFi 下连接的设备，还可以扫描 ip 端口。

#### Simple课程表

[Simple课程表](https://www.coolapk.com/apk/com.strivexj.timetable) 一个简约的课程表，就是导入课程要一个一个输……

#### WPS Pro

[WPS Pro](https://www.coolapk.com/feed/18863779?shareKey=ZTk0MTI1NmU1NjUzNWVjNjQwODk~) 需要 [激活码](https://shenzjd.com/archives/2290#comments)，激活了后无广告。

### 在线内容

#### bilibili

[bilibili](https://coolapk.com/apk/tv.danmaku.bili) 不解释。play store 上有更简洁的版本，就不放上来了。话说，不来关注一下我？[@ToddZhou](https://space.bilibili.com/14158798)

#### Apple Music

[Apple Music](https://coolapk.com/apk/com.apple.android.music) 苹果主要是国外音乐很全，而且界面够简洁好看。需要付费才能用，学生党每月￥5也不贵，非学生每月￥10。

#### OpenHub

[OpenHub](https://github.com/ThirtyDegreesRay/OpenHub/releases) Github 的第三方客户端，用起来还蛮顺手，就懒得换官方了。

