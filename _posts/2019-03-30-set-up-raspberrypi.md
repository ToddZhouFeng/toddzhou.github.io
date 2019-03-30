---
layout: post
title:  树莓派初始配置及软件安装
date:   2019-03-30 09:29:00 +0800
categories: document
tag: Raspberry Pi/Arduino
music-id: 465675773
---

* content
{:toc}
# 简介

&emsp;&emsp;树莓派安装完系统后，总是需要设置一堆东西，然后还要安装各种软件，等等。而且我经常把树莓派玩坏......重装系统是常有的事。我受够了每次都要上网查一堆资料，下面我将总结一下所有东西。



# 基本设置



##第一次开机

&emsp;&emsp;我的树莓派是3B/3B+，系统为`2018-11-13-raspbian-stretch.img`，也就是带桌面但没那么多软件那个。安装系统只需要10分钟。

&emsp;&emsp;第一次开机时间有点久。进入桌面后，先设置Country(China)，Language(Chinese)，Timezone(Shanghai)，勾选Use US keyboard。接着输入‘pi’用户新的密码。然后连接WiFi。再更新软件，时间有点久，而且容易出错，跳过。

&emsp;&emsp;重启。

&emsp;&emsp;树莓派现在对中文支持较好，重启后并不会出现中文乱码。赞！



## 换源

&emsp;&emsp;众所周知，树莓派官方软件源在中国很慢，所以要换源。编辑软件源list文件：

~~~shell
sudo nano /etc/apt/sources.list
~~~

&emsp;&emsp;注释官方源，然后在下面选一个输进去：

~~~shell
#清华大学
deb http://mirrors.tuna.edu.cn/raspbian/raspbian/ stretch main contrib non-free
deb-src http://mirrors.tuan.edu.cn/raspbian/raspbian/ stretch main contrib non-free

#中国科技大学
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free

#阿里云
deb http://mirrors.aliyun.com/raspbian/raspbian/ stretch main contrib non-free
deb-src http://mirrors.aliyun.com/raspbian/raspbian/ stretch main contrib non-free
~~~

&emsp;&emsp;修改系统更新源：

~~~shell
sudo nano /etc/apt/sources.list.d/raspi.list
~~~

&emsp;&emsp;同样，注释掉官方源，换成下面的某一个：

~~~shell
#清华大学
deb http://mirrors.tuan.edu.cn/archive.raspberrypi.org/debian/ stretch main ui

#中国科技大学
deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ stretch main ui

#阿里云
deb http://mirrors.aliyun.com/archive.raspberrypi.org/debian/ stretch main ui
~~~

&emsp;&emsp;然后执行：

~~~shell
sudo apt-get update && sudo apt-get upgrade -y
~~~



