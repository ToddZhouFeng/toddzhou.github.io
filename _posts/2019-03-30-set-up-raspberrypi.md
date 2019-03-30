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



## 换源&更新

&emsp;&emsp;众所周知，树莓派官方软件源在中国很慢，所以要换源。编辑软件源list文件：

~~~shell
sudo nano /etc/apt/sources.list
~~~

&emsp;&emsp;注释官方源，然后在下面选一个输进去：

~~~shell
#清华大学
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free

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

&emsp;&emsp;同样，注释掉官方源，换成下面这个：

~~~shell
#中国科技大学
deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ stretch main ui
~~~

&emsp;&emsp;然后执行：

~~~shell
sudo apt-get update && sudo apt-get upgrade -y
~~~

&emsp;&emsp;等待漫长的软件更新吧！你可以去我博客看看其他文章先，等更新完成后再继续下面步骤。



## 换pip源

&emsp;&emsp;换成国内pip源：

~~~shell
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
~~~

&emsp;&emsp;上面是清华源，其他源也行：

~~~shell
#阿里云
http://mirrors.aliyun.com/pypi/simple/
#中国科技大学
https://pypi.mirrors.ustc.edu.cn/simple/
#豆瓣(douban)
http://pypi.douban.com/simple/
#中国科学技术大学
http://pypi.mirrors.ustc.edu.cn/simple/
~~~



## 必备软件

&emsp;&emsp;在前面加`sudo apt-get install`来安装下面的软件。

~~~shell
ttf-wqy-zenhei #文泉驿的中文字体
scim-pinyin #中文输入法，重启生效
vim #命令行的代码编辑器
cmake #跨平台的自动化建构系统
nestopia #玩NES游戏
smplayer #媒体播放器，VLC太容易卡死了
~~~



## 必备Python库

&emsp;&emsp;在前面加`sudo pip install`来安装下面的库。

~~~shell

~~~



## 杂七杂八

&emsp;&emsp;树莓派3.5mm输出口有底噪，可在`/boot/config.txt`文末加入一行：

~~~shell
audio_pwm_mode = 2
~~~

&emsp;&emsp;保存并重启，音质有少量提升。



---





# 深度学习

## opencv3.4

&emsp;&emsp;安装numpy：

~~~shell
sudo pip3 install numpy
~~~

&emsp;&emsp;扩大micro SD卡空间：进入`raspi-config`，选择`Advanced Options`，选择`Expand Filesystem`，重启。

&emsp;&emsp;安装所需要的库（一次一行，别一次全装）：

~~~shell
sudo apt-get install build-essential git cmake pkg-config -y
sudo apt-get install libjpeg8-dev -y
sudo apt-get install libtiff5-dev -y
sudo apt-get install libjasper-dev -y
sudo apt-get install libpng12-dev -y
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev -y
sudo apt-get install libgtk2.0-dev -y
sudo apt-get install libatlas-base-dev gfortran -y
~~~

&emsp;&emsp;新建一个文件夹叫opencv，并进入该文件夹

~~~shell
sudo mkdir opencv
cd opencv
~~~





