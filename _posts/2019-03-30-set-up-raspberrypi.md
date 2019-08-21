---
layout: post
title:  树莓派初始配置及软件安装
date:   2019-03-30 09:29:00 +0800
categories: document
tag: Raspberry Pi
music-id: 465675773
---





> 树莓派安装完系统后，总是需要设置一堆东西，然后还要安装各种软件，等等。而且我经常把树莓派玩坏......重装系统是常有的事。我受够了每次都要上网查一堆资料，下面我将总结一下所有东西。

<!-- more -->

# 基本设置



## 第一次开机

&emsp;&emsp;我的树莓派是3B/3B+，系统为`2018-11-13-raspbian-stretch.img`，就是带桌面但没那么多软件那个。烧录系统只需要10分钟。（最新的系统是`buster`，有些内容不兼容）

&emsp;&emsp;第一次开机时间有点久。进入桌面后，先设置Country(China)，Language(Chinese)，Timezone(Shanghai)，勾选Use US keyboard。接着输入‘pi’用户新的密码。然后连接WiFi。不要更新软件，更新的话时间有点久，而且容易出错，跳过。

&emsp;&emsp;重启。

&emsp;&emsp;树莓派现在对中文支持较好，重启后并不会出现中文乱码。赞！



## 换源&更新

### Stretch

&emsp;&emsp;众所周知，树莓派官方软件源在中国很慢，所以要换源。编辑软件源list文件：

~~~shell
sudo nano /etc/apt/sources.list
~~~

&emsp;&emsp;注释官方源，然后在下面选一个输进去：

~~~shell
#清华大学
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi

#中国科技大学
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi

#阿里云
deb http://mirrors.aliyun.com/raspbian/raspbian/ stretch main contrib non-free rpi
deb-src http://mirrors.aliyun.com/raspbian/raspbian/ stretch main contrib non-free rpi
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

&emsp;&emsp;等待漫长的软件更新吧！（至少要半个小时）你可以去我博客看看其他文章先，等更新完成后再继续下面步骤。



### Buster

&emsp;&emsp;Raspbian于2019/06/12升级到了Buster，基于目前最新的Debian. 换源方法是一样的，只不过源改为：

~~~bash
#/etc/apt/sources.list
#中科大
deb http://mirrors.ustc.deu.cn/raspbian/raspbian/ buster main contrib non-free rpi
deb-src http://mirrors.ustc.deu.cn/raspbian/raspbian/ buster main contrib non-free rpi
~~~

~~~bash
#/etc/apt/sources.list.d/raspi.list
#中科大
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ buster main ui
~~~

&emsp;&emsp;实际上，就只是把 Stretch源 中的 `stretch` 换成了 `buster`



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
ttf-wqy-microhei #文泉驿的中文字体#Jussie和lite才需安装
scim-pinyin #中文输入法，重启生效
vim #命令行的代码编辑器
cmake #跨平台的自动化建构系统
nestopia #玩NES游戏#不玩就不安装
smplayer #媒体播放器，VLC太容易卡死了
~~~

&emsp;&emsp;还有一些软件要通过安装包安装：

1. 对rar解压支持（貌似有错误:(

   ~~~shell
   cd ~/Download
   sudo wget http://rarlab.com/rar/rarlinux-3.8.0.tar.gz
   tar zxvf rarlinux.3.8.0.tar.gz
   cd rar
   make && make install
   ~~~



## 必备Python库

&emsp;&emsp;在前面加`sudo pip3 install`来安装下面的库。

~~~shell
numpy #数学计算 #如果出错，装上libatlas-base-dev
matplotlib #可视化
nltk #自然语言处理
jieba #中文语言处理
scipy #科学计算
pandas #数据分析#需要 cython 库#如果出错，用sudo apt-get install python-pandas安装
statsmodels #统计模型
scikit-learn #机器学习
scrapy #爬虫
~~~



## 杂七杂八

### 音质提升

&emsp;&emsp;树莓派3.5mm输出口有底噪，可在`/boot/config.txt`文末加入一行：

~~~shell
audio_pwm_mode = 2
~~~

&emsp;&emsp;保存并重启，音质有少量提升。顺便说一句，3B+播放时如果把音量调为0会听到明显的电流声，3B反而没有，3B的音质竟然比3B+好，因吹斯听。



### 玩具

```shell
cmatrix #黑客屏保
figlet #ASCII字符画
```





# 深度学习



## Jupyter Notebook

&emsp;&emsp;先升级 pip ，否则会出现 unsupported operand types ...，逐个输入下面的命令：

```shell
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
python3 get-pip.py
```

&emsp;&emsp;然后执行下面这个命令：

~~~bash
sudo pip3 install jupyter
~~~

&emsp;&emsp;如果出现安装不成功的情况，可以逐个安装依赖的库（建议多试几次上面，实在不行再逐个安装）：

~~~shell
sudo pip3 install ipython-genutils decorator traitlets tornado pyzmq backcall pygment ptyprocess pexpect pickleshare parso jedi wcwidth prompt-toolkit ipython ipykernel jinja2 attrs pyrsistent jsonschema nbformat entrypoints testpath webencodinga bleach defusedxml pandocfilters mistune nbconvert Send2Trassh terminado notebook ipywidgets qtconsole
~~~

&emsp;&emsp;安装好后，输入下面命令生成配置文件：

```shell
jupyter notebook --generate-config
```

&emsp;&emsp;修改配置文件：

```shell
sudo nano ~/.jupyter/jupyter_notebook_config.py
```

&emsp;&emsp;修改内容如下：

```shell
#c.NotebookApp.ip = 'localhost'
改为
c.NotebookApp.ip = '*'
```

```shell
#c.NotebookApp.open_browser = True
改为
c.NotebookApp.open_browser = False
```

```shell
#c.NotebookApp.port = 8888
改为
c.NotebookApp.port = 8888
```

```shell
#c.NotebookApp.notebook_dir = ''
改为
c.NotebookApp.notebook_dir = '/home/pi'
```

&emsp;&emsp;配置完成后修改密码：

```shell
jupyter notebook password
```

&emsp;&emsp;然后就可以使用Jupyter Notebook了，方法是在树莓派上输入`jupyter notebook`，然后在电脑上的浏览器中打开`树莓派的ip地址:8888`



## Opencv 3.4.0

> 参考博客[【树莓派】树莓派+OpenCV3.4 + python3.5 成功以及注意细节](https://www.jianshu.com/p/3180a253fe3c)，安装后占4.8GB，如果空间不足就别装

&emsp;&emsp;安装numpy：

~~~bash
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
mkdir opencv
cd opencv
~~~

&emsp;&emsp;从Github上下载安装文件

~~~shell
wget https://github.com/Itseez/opencv/archive/3.4.0.zip
wget https://github.com/Itseez/opencv_contrib/archive/3.4.0.zip
~~~

&emsp;&emsp;解压这两个文件

~~~shell
sudo unzip 3.4.0.zip 3.4.0.zip.1
~~~

&emsp;&emsp;创建build文件夹，并进入

~~~shell
cd opencv-3.4.0
mkdir build
cd build
~~~

&emsp;&emsp;然后开始cmake，以下命令在同一行，但不要急！看命令后面的解释：

~~~shell
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D OPENCV_EXTRA_MODULES_PATH=/home/pi/opencv/opencv_contrib-3.4.0/modules -D BUILD_EXAMPLES=ON -D WITH_LIBV4L=ON PYTHON3_EXECUTABLE=/usr/bin/python3.5 PYTHON_INCLUDE_DIR=/usr/include/python3.5 PYTHON_LIBRARY=/usr/lib/arm-linux-gnueabihf/libpython3.5m.so PYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3/dist-packages/numpy/core/include ..
~~~

&emsp;&emsp;**千万要注意，上面的路径不要写错！**最好去看看有没有对应的文件或文件夹，也就是下面几个

~~~shell
OPENCV_EXTRA_MODULES_PATH=/home/pi/Downloads/opencv_contrib-3.4.0/modules #解压出来的东西
PYTHON_INCLUDE_DIR=/usr/include/python3.5 
PYTHON_LIBRARY=/usr/lib/arm-linux-gnueabihf/libpython3.5m.so 
PYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3/dist-packages/numpy/core/include #numpy的安装路径
~~~

&emsp;&emsp;确保无误后再开始cmake

&emsp;&emsp;最后开始编译，欲速则不达，不要用make -j4等加速

~~~shell
sudo make && sudo make install
~~~

&emsp;&emsp;等四五个小时后就安装完成了。



## Tensorflow 

> 参考博客[从零开始：在树莓派上构建tensorflow——详细至极](https://blog.csdn.net/qq_38960810/article/details/78640171)，他装的是1.1.0，我装的是1.13.1。安装后占200多M

&emsp;&emsp;先下载这些库，其实你不安装也行，待会安装tensorflow时它会自动安装，但那时容易出错。

~~~shell
sudo pip3 install grpcio protobuf numpy tensorboard gast termcolor astor keras absl-py futures enum markdown h5py mock pbr
~~~

&emsp;&emsp;如果某个库，比如 h5py 装不了，可以去浏览器下载：[https://piwheels.org/simple/h5py/h5py-2.9.0-cp35-cp35m-linux_armv7l.whl](https://piwheels.org/simple/h5py/h5py-2.9.0-cp35-cp35m-linux_armv7l.whl)（这个链接就是 pip3 install 之后出现的链接），然后执行：

~~~shell
cd /home/pi/Downloads/ #去到下载目录
sudo pip3 install h5py-2.9.0-cp35-cp35m-linux_armv7l.whl
~~~

&emsp;&emsp;其他安装不了的库也这样安装。



&emsp;&emsp;如果上网的姿势很科学，可以直接：

~~~shell
sudo pip3 install tensorflow
~~~

&emsp;&emsp;如果你觉得上面的速度太慢了，可以去浏览器下载（或用wget）：[https://piwheels.org/simple/tensorflow/tensorflow-1.14.0-cp35-none-linux_armv7l.whl](https://piwheels.org/simple/tensorflow/tensorflow-1.14.0-cp35-none-linux_armv7l.whl)，然后执行：

~~~shell
cd /home/pi/Downloads #去到下载目录，如果用wget则不用这行
sudo pip3 install tensorflow-1.14.0-cp35-none-linux_armv7l.whl
~~~

&emsp;&emsp;然后就等等等等15~30分钟吧。



&emsp;&emsp;下面我们试试图像识别，运行下面的命令：

~~~shell
cd && mkdir tensorflow
cd tensorflow/
wget https://raw.githubusercontent.com/tensorflow/models/tutorials/image/imagenet/classify_image.py
~~~

&emsp;&emsp;然后你就会在/home/pi/tensorflow文件夹里看到一个classify_image.py，运行该程序，它会自动下载图像识别库（85M）（只有第一次会下载，以后就不用了）。

&emsp;&emsp;等到它下载完成后，通过下面命令进行图像识别：

~~~shell
python3 /home/pi/tensorflow/classify_image.py --image_file <图片路径>
~~~

&emsp;&emsp;比如我在网上找了一张hamster图片：

![仓鼠](https://static.wamiz.fr/images/animaux/rongeurs/crop/medium/hamster.jpg)

&emsp;&emsp;运行后显示的结果是（效果还行）：

~~~shell
hamster (score = 0.70514)
mink (score = 0.00465)
polecat, fitch, foulmart, foumart, Mustela putorius (score = 0.00370)
black-footed fettet, ferret, Mustela nigripes (score = 0.00274)
skunk, polecat, wood pussy (score = 0.00266)
~~~

&emsp;&emsp;貌似只能识别动物和物品（试了一下黄色图片，识别不了）。

