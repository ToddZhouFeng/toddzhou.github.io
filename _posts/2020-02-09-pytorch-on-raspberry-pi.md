---
layout: post
title:  树莓派上安装 PyTorch
date:   2020-02-09 17:16:00 +0800
tag: [Raspberry Pi, Linux, PyTorch, Tutorial]
music-id: 28391158
---



> 买不起 jetson nano，难过……

<!-- more -->



# 前言

&emsp;&emsp;Raspbian 源上的 PyTotch 较老，要用 1.0 以上的库必须自己编译才行。然而 arm 版的 PyTorch 有很多不支持，需要比较复杂的配置才行。所以记录一下我的安装过程。



# 增加 SWAP

```bash
sudo nano /etc/dphys-swapfile
```

修改 CONF_SWAPSIZE 和 CONF_MAXSWAP，把后面的数字改为 2048 以上（2GB），然后使 SWAP 空间生效：

```bash
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```

通过下面命令确认：

```bash
free -h
```





# 依赖

```bash
sudo apt install libopenblas-dev libblas-dev libatlas-dev m4 cmake cython3 python3-dev python3-yaml python3-cffi python3-setuptools
```

```bash
pip install numpy pyyaml
```



# 获取源码

```bash
git clone https://github.com/pytorch/pytorch.git
cd pytorch
git checkout v1.2.0
git submodule update --init --recursive
```

1.2.0使用的protobuf有一个Bug会导致报undefined reference to `__atomic_fetch_add_8'的错误，参考[Pytorch Issue](https://link.zhihu.com/?target=https%3A//github.com/pytorch/pytorch/issues/22564)，升级protobuf到最新即可解决：

```bash
git submodule update --remote third_party/protobuf
```



# 设置参数

```bash
export NO_CUDA=1
export NO_DISTRIBUTED=1
export NO_MKLDNN=1
export NO_NNPACK=1
export NO_TEST=1
export MAX_JOBS=2 #编译时最大CPU核心数
```



# 编译

```bash
python3 setup.py build
```

我的树莓派3B+ 编译了30多个小时 😭，我看的所有教程都说只需要 7~8个小时。

编译时可能会出现 `subprocess.CalledProcessError: Command '['cmake', '--build', '.', '--target', 'install', '--config', 'Release', '--', '-j', '4']' returned non-zero exit status 2. `，这时要检查一下前面的东西。如果反复出现，考虑一下用 Python3.6 而不是 3.7（buster 默认是 3.7）

# 安装

```bash
sudo -E python3 setup.py install
```



# 测试

```python
from __future__ import print_function
import torch
a = torch.eye(3,3)
print (a)
```

显示结果：

```python
tensor([
    [1,1,1],
    [1,1,1],
    [1,1,1]
])
```



# 参考

* [http://www.neko.ne.jp/~freewing/raspberry_pi/raspberry_pi_build_pytorch_deep_learning_framework/](http://www.neko.ne.jp/~freewing/raspberry_pi/raspberry_pi_build_pytorch_deep_learning_framework/)
* [https://medium.com/hardware-interfacing/how-to-install-pytorch-v4-0-on-raspberry-pi-3b-odroids-and-other-arm-based-devices-91d62f2933c7](https://medium.com/hardware-interfacing/how-to-install-pytorch-v4-0-on-raspberry-pi-3b-odroids-and-other-arm-based-devices-91d62f2933c7)
* [https://zhuanlan.zhihu.com/p/57938855](https://zhuanlan.zhihu.com/p/57938855)
* [https://zhuanlan.zhihu.com/p/79807661](https://zhuanlan.zhihu.com/p/79807661)


