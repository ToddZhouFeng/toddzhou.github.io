---
layout: post
title:  Raspbian版SCUT树莓派搭建宿舍无线网
date:   2019-03-27 16:26:00 +0800
categories: document
tag: Raspberry Pi/Arduino
music-id: 465675773
---

* content
{:toc}




> &emsp;&emsp;首先我要特别感谢[象牙塔塔主](<https://www.jianshu.com/u/e21ceff836ec>)，他的文章是我在网上查到的唯一资料，为了向他致敬，我决定沿用他的主标题（希望这不要被认为是蹭热度......），并且我这篇教程只允许他转载。希望各位同学去他的[简书](https://www.jianshu.com/u/e21ceff836ec)关注并点赞。
>
> &emsp;&emsp;同时感谢[scutclient](https://github.com/scutclient)，他们才是真正的大神。
>
> &emsp;&emsp;有任何问题，请联系我：Todd310378072@outlook.com



# 为什么多此一举？

&emsp;&emsp;网上已经有相关教程了：[SCUT树莓派宿舍搭建无线网](<https://www.jianshu.com/p/4b8e98475aeb>)，那个教程写得很好，但我在实践的过程中遇到了问题：

* Ubuntu对树莓派3B+的支持不是很好，官网上提供的系统根本用不了。
* Ubuntu太过于臃肿，树莓派孱弱的性能不是很够，而且Ubuntu耗电貌似大一点。

&emsp;&emsp;于是我决定找到在Debian系统上开WiFi的方法。显然，我找到了，已经用上了哦。但，这个配置的过程十分漫长......请做好准备（其实也就半小时）。



# 准备

* 校园网账号（打开微信，我的企业：华南理工大学，校园网，右下营业厅，个人信息。）
* 安装好Debian的树莓派（以及屏幕、键盘、鼠标、电源）
* 网线
* 网络（我们需要下载一些东西）



# 连接SCUT有线网

&emsp;&emsp;首先安装`cmake`，输入下面命令：

~~~shell
sudo apt-get install cmake
~~~



&emsp;&emsp;按`Ctrl`+`Alt`+`t`打开命令行，**依次**输入以下命令：

~~~shell
git clone https://github.com/scutclient/scutclient.git
cd scutclient
mkdir build && cd build
sudo cmake ..
sudo make
~~~

&emsp;&emsp;然后右键点击右上角的WiFI图标，点第一个`Wireless & Wired Network Setting`，会弹出下面界面：

![网络设置](http://www.web3.lu/wp-content/uploads/2016/02/network_pref.jpg "网络设置")

&emsp;&emsp;点击右上角的`wlan0`，换成`eth0`，对照着下面的指示填（相关信息在你注册校园网时的界面上，或微信校园网的个人信息那里）：

~~~s&#39;sh
IPv4 Address: #填IP地址
IPv6 Address: #空
Router: #填网关
DNS Servers: #填主DNS或备DNS都行
~~~

&emsp;&emsp;勾选上面的`Automatically configure empty options`，再点下面的`Apply`，然后关闭窗口。



&emsp;&emsp;然后，在桌面上右键新建一个`Empty File`，名为`connect.sh`，里面的内容如下（将`<>`内的内容**连同**`<>`替换掉！）：

~~~shell
sudo ifconfig eth0 down
sudo ifconfig eth0 <IP地址> netmask <掩码>
sudo ifconfig eth0 hw ether <MAC地址>
sudo route add default gw <网关>
sudo ifconfig eth0 up
sudo nohup sudo /home/pi/scutclient/build/scutclient --username <SCUT账号> --password <SCUT密码>
~~~

&emsp;&emsp;然后保存。



&emsp;&emsp;然后接上网线，在命令行里运行（执行这个`connect.sh`文件）：

~~~shell
sudo sh /home/pi/Desktop/connect.sh
~~~

&emsp;&emsp;打开树莓派的浏览器测试一下，看看能不能访问百度。如果不能，执行下面这步；如果行，请跳到下一标题。



---

&emsp;&emsp;执行下面的命令：

~~~shell
sudo chmod 777 /etc
sudo chmod 777 /etc/resolv.conf
~~~

&emsp;&emsp;打开`/etc/resolv.conf`，把`nameserver`后面的数字改成`114.114.114.114`，注意`nameserver`和数字之间有一个空格。



&emsp;&emsp;再看看浏览器能不能访问到百度，如果能，执行下面步骤：

&emsp;&emsp;把改好的`/etc/resolv.conf`复制到桌面，然后在命令行输入：

~~~shell
sudo nano /etc/rc.local
~~~

&emsp;&emsp;然后命令行会进入编辑界面，在下面，`exit 0`这一行的前面加上一行：

~~~shell
sudo cp -f /home/pi/Desktop/resolv.conf /etc/resolv.conf
~~~

&emsp;&emsp;然后按`Ctrl`+`x`，再输入`y`，再回车。至此就完成了第一部分。



# 创建热点

&emsp;&emsp;参考了csdn上一篇[博客](<https://blog.csdn.net/u014271612/article/details/53766627>)

&emsp;&emsp;打开命令行，**依次**执行下面命令。`#`开头的是注释：

~~~shell
#将代码copy到本地，安装
git clone https://github.com/oblique/create_ap
cd create_ap
sudo make install

#安装依赖的库
sudo apt-get install util-linux procps hostapd iproute2 iw haveged dnsmasq
~~~



&emsp;&emsp;然后我们每次开热点，都可以用如下命令：

~~~shell
sudo create_ap wlan0 eth0 热点名 密码
~~~

&emsp;&emsp;它有时候会提示出错，这可能是因为你的`wlan0`已经连接了WiFI，试一下把断开WiFi再执行，或者重启再执行。

&emsp;&emsp;拿手机试一下能不能连接成功并上网，如果能，我们可以开始配置开机自启，如果不能，检查一下上面步骤有没有出错。实在不行请联系我。



# 开机自启

&emsp;&emsp;在命令行输入：

~~~shell
sudo nano /etc/rc.local
~~~

&emsp;&emsp;然后命令行会进入编辑界面，在下面，`exit 0`这一行的前面加上下面几行，`#`开头的注释可以不写：

~~~shell
# 下面是connect.sh的内容，用鼠！标！复制粘贴到这里
sudo ifconfig eth0 down
sudo ifconfig eth0 <IP地址> netmask <掩码>
sudo ifconfig eth0 hw ether <MAC地址>
sudo route add default gw <网关>
sudo ifconfig eth0 up

#开机启动WiFi
sudo create_ap wlan0 eth0 热点名 密码
~~~

&emsp;&emsp;如果开机没有启动WiFi，在sudo后面加个 nohup ：

```shell
sudo nohup create_ap wlan0 eth0 热点名 密码
```

&emsp;&emsp;上面这条命令是将输出重定向到nohup文件中，并且即使关闭了当前shell也不会终止该进程。

&emsp;&emsp;这样虽然开机有WiFi，但由于没登陆SCUT，所以还是没网的，你还要执行上面的`connect.sh`文件。不过你可以不用连接屏幕和键盘，直接用SSH就行了。关于SSH的教程，网上有很多，这个就留给Geek的你吧！



# 防蹭网

&emsp;&emsp;校园网的速度有限，估计一个宿舍4个人用是没问题的，但如果再多几个人，速度就捉急了。而且，估计宿舍里的其他人很大概率会把WiFi密码分享出去，所以，虽然这样有点自私，但还是加个防蹭网措施吧！就是Mac地址白名单！

&emsp;&emsp;先在手机上下一个Fing（安卓/iOS都有），然后连接你的WiFi，点击Fing右上角的刷新，然后你就可以看到目前有哪些设备连接了你的WiFi，随便点进一个设备，下方有一个“MAC地址”，长按可以复制。将白名单设备全部连到这个WiFi，然后将它们的Mac地址复制下来。

&emsp;&emsp;回到我们的树莓派。输入

~~~shell
sudo nano /etc/hostapd/hostapd.accept
~~~

&emsp;&emsp;然后将Mac地址打上去，每个Mac地址占一行。保存并退出：按`Ctrl+x`，`y`，回车。

​	原来的开启热点的命令是：

~~~shell
sudo create_ap wlan0 eth0 热点名 密码
~~~

&emsp;&emsp;换成：

~~~shell
sudo create_ap --mac-filter wlan0 eth0 热点名 密码
~~~

&emsp;&emsp;重新启动热点即可生效。

&emsp;&emsp;后期需要添加设备的话，可以先用手机或电脑开热点，把设备连接到热点，然后在手机或电脑上看Mac地址，再添加到`/etc/hostapd/hostapd.accept`（超级麻烦，所以如果是别的宿舍来借网，我都是直接用回原来的命令开热点）



# 优化

&emsp;&emsp;WiFi信号在信道上传播，如果某个信道有多个WiFi，则信号间可能会有影响。我们可以将热点换到WiFi较少的信道即可。如何查看信道呢？你可以上网查一下，我用的是CloudWalker（安卓）这个软件。

&emsp;&emsp;修改信道的方法很简单，在启动命令中加`-c 信道`，即：

~~~shell
sudo create_ap -c 信道 wlan0 eth0 热点名 密码
~~~

&emsp;&emsp;如果要兼顾防蹭网，则输入：

~~~shell
sudo create_ap -c 信道 --mac-filter wlan0 eth0 热点名 密码
~~~

&emsp;&emsp;我用的是信道10，以供参考。



# 结语

&emsp;&emsp;虽然看起来很多要做，但对于熟悉命令行的同学来说，这点命令也不算多。我按照这个实践过后已经成功了，如果你有任何问题，请联系我！我的邮箱为310378072@qq.com。

&emsp;&emsp;P.s. 树莓派配置完后，如果断开热点，并通过WiFi连接网络，可能会连接不了。我初步猜测可能是掩码的问题，但懒得研究怎么弄...于是采取了个简单粗暴的方法`sudo ifconfig eth0 down`，然后就连上了。

&emsp;&emsp;P.P.s. 3B和3B+我都试了弄热点，虽然没仔细测过，个人感觉在供电充足的情况下，3B+会比3B快一点。但由于3B+需要2.5A电流（普通的充电器达不到，快充没协议也达不到），所以3B反而好一丢丢。



2019/5/20补充：访问不了网页端斗鱼的解决方案：挂VPN。（学校貌似禁了斗鱼）



# 未来的安排：

1. 学校的有线网是支持ipv6的，希望能够让热点也支持ipv6
2. 树莓派的硬件实在不是作为路由器的料，以后还是转到路由器+openwrt吧

