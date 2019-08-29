---
layout: post
title:  openwrt搭建宿舍无线网
date:   2019-08-26 18:42:00 +0800
tag: [Linux, Diary, Tutorial]
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
* ssh软件（如果没有，文末有推荐软件）



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

然后新建目录：

```bash
mkdir package/scutclient
```

从 [https://github.com/scutclient/scutclient](https://github.com/scutclient/scutclient) 下载仓库里面的 /openwrt/Makefile 文件（貌似只能全部下载再解压？），然后放到上面新建的目录里面。

然后运行：

```bash
make defconfig
make package/scutclient/compile V=s
```

如果出现错误，可能缺少什么软件（比如我是缺少一个 gawk ），或者上面的 sdk 有错（我前几次都编译失败，换了那个sdk才成功）

编译好后，将编译好的 ipk文件（在 bin 文件夹里）放到openwrt路由器里，然后通过 `opkg install [文件]` 来安装。



# 连接网络

将路由器插上网线，然后连接校园网：

```bash
scutclient --username <username> --password <password>
```

上面那个大概率会出错，因为需要指定是哪个 WAN 口：

```bash
scutclient --username <username> --password <password> -i <wan>
```

如果你不知道是哪个，就用 `ifconfig` 查看，然后都试一次。

如果成功的话，就可以将这个命令挂到后台：（先 ctrl + c 停止正在运行的命令）

```bash
nohup scutclient --username <username> --password <password> -i <wan>
```

由于每天都会断网，所以早上起来时需要重新运行这个命令。



# 配置 wifi

这个就不用说了，wifi 名随便写，最好不要写自己宿舍号（也不要~~像我那样~~写其他宿舍号），担心学校有人查，如果能隐藏 wifi 就最好了。



# 谷歌学术

淘宝店帮忙刷的openwrt 大概率会安装 Shadowsocks，比如我的就安装了 ShadowsockR Plus +。如果你的没安装，自己上网找教程。我说一下大概的配置：

* 添加服务器（就填好ip、端口、密码就行），启用“TCP快速打开”和“自动切换”
* 客户端，主服务器就选添加的服务器，运行模式选绕过大陆ip，其余可以不变
* 保存并启动



# 推荐软件

如果你没有 ssh，我推荐两个：

* 手机端：ConnectBot
* 电脑端：MobaXterm