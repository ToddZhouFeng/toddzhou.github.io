---
layout: post
title:  Raspbian版SCUT树莓派搭建宿舍无线网
date:   2019-08-26 18:42:00 +0800
tag: [Raspberry Pi, Linux, Diary]
music-id: 28391158
---



> 自己动手，丰衣足食。

<!-- more -->

> 之前已经有用树莓派搭过路由器了，为了追求更快的网速，这次决定用OpenWrt系统的路由器来试试。



# 准备

* 一台装有 openwrt 的路由器
* 校园网账号
* 网线
* 一台装有 Linux 的 x86_64 设备（电脑/虚拟机/服务器，我在教程中用的是服务器）



# 交叉编译

由于 OpenWrt 路由器并没有 cmake 和 make，所以需要先在 Linux服务器上编译好后，再部署到路由器上。

先通过 ssh 连接路由器，然后输入：

```bash
cat /etc/openwrt_release
```

然后会显示一些信息，这是我的：

```bash
DISTRIB_ID='OpenWrt'
DISTRIB_RELEASE='18.06.1'
DISTRIB_TARGET='ar71xx/nand' #重要的是这一行！必须是ar71xx
DISTRIB_ARCH='mips_24kc'
DISTRIB_TAINTS='no-all'
DISTRIB_REVISION='R8.1.11 By Lean'
DISTRIB_DESCRIPTION='OpenWrt '
```

如果不是`ar71xx`，那你需要自行找相应的 target 来编译；如果是，ssh 切换到 Linux服务器，输入：

```bash
wget https://downloads.openwrt.org/snapshots/targets/ar71xx/generic/openwrt-sdk-ar71xx-generic_gcc-7.4.0_musl.Linux-x86_64.tar.xz
```

> 注：由于 openwrt每年都会更新，所以上面那个文件不存在的话，去 https://downloads.openwrt.org/snapshots/targets/ar71xx/generic/ 找最新的来替换。

下载好后，解压并进入相应目录：

```bash
tar -Jxvf openwrt-sdk-ar71xx-generic_gcc-7.4.0_musl.Linux-x86_64.tar.xz
cd openwrt-sdk-ar71xx-generic_gcc-7.4.0_musl.Linux-x86_64
```

然后