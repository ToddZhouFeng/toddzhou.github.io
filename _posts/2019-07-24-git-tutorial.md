---
layout: post
#标题配置
title:  git入门
#时间配置
date:   2019-07-24 22:35:13 +0800
#大类配置
categories: document
#小类配置
tag: [Tutorial]
#网易云音乐，只能播放无版权保护的
music-id:  465675773
---

> 大致介绍一下git怎么用

<!-- more -->

# Git 历史

&emsp;&emsp;Git 的诞生和 Linux 系统密切相关，Linux 开发它就是为了 Linux 这种大规模的项目服务的，这就使得它有着很多优点，比如：
* **免费**
* **快速**
* **简单**
* **分布式**
* **多分支**

&emsp;&emsp;学会用 Git 可以帮助我们更好地进行版本控制和团队协作。你可以在网上搜搜关于版本控制的其他方法，再对比一下 Git，就会懂 Git 是多么的好用了！



# 学习资源

&emsp;&emsp;一般学习资源都是放在最后才说，但为了方便各位查找，故放前面。以下是我学习 git 的参考，分享一下：

1. [Git 教程\|菜鸟教程](https://www.runoob.com/git/git-tutorial.html)
2. [猴子都能懂的GIT入门 \| 贝格乐（Backlog）](https://backlog.com/git-tutorial/cn/)
3. [Git Cheat Sheet \| Github](https://github.github.com/training-kit/downloads/zh_CN/github-git-cheat-sheet/)
4. [Git Tutorial](https://www.tutorialspoint.com/git/)

&emsp;&emsp;当然，最重要的是官方网站：[Git](https://git-scm.com/)



# 安装

Debian/Ubuntu：

```bash
sudo apt-get install git
```

Windows：

[官网](https://git-scm.com)/[镜像站](https://github.com/waylau/git-for-win)，至于 GUI，这个看个人喜好吧，一般都是用 Github Desktop.

Mac：

听说是安装 Xcode，再用 Xcode 安装。没有设备，不知谁能赞助一部。



# Git 的一些概念

&emsp;&emsp;我第一次看 Git 的教程，就被下面三个概念搞晕了

* 工作区 Working Directory
* 暂存区 Staging Area
* Git 仓库 Repository

&emsp;&emsp;直接解释这仨反而不太好理解，不妨直接说说用 Git 管理的流程：

1. 在 *工作区* 中修改代码
2. 将代码存到 *暂存区*
3. 将 *暂存区* 中的代码提交到 *Git 仓库*

&emsp;&emsp;那么为啥不直接将代码提交到 *Git 仓库* 呢？因为一般我们只有在软件完善之后才将代码提交到 *Git 仓库* ，在那之前，我们将那些已完善的文件先暂时存在 *暂存区*。

&emsp;&emsp;与流程相对应，我们的每个文件都属于下面三种状态的一种：

1. 已修改
2. 已暂存
3. 已提交

&emsp;&emsp;下面就讲讲怎么实现这个流程。



# 新建一个仓库

将当前目录作为 git 仓库：

```bash
git init
```

将指定目录作为仓库：

```bash
git init [directory]
```



# 添加到暂存区

将当前的所有文件添加到暂存区：

```bash
git add .
```

将指定文件添加到暂存区：

```bash
git add [file]
```

添加完后，我们可能要查看一下暂存区中有哪些文件，可以用：

```bash
git status -s #加s参数用于获取简短结果
```

这个命令会显示所有文件以及它们的状态（显示在文件名前面）：`??` 即未添加，`A` 即已添加到暂存区，`M` 即已修改，`AM` 即添加后又有修改



# 添加到 Git 仓库

提交前，先填好用户名和邮箱地址：

```bash
git config --global user.name 'blablabla'
git config --global user.email bla@mail.com
```

然后执行下面步骤进行提交：

```bash
git commit
```

然后 Git 就会打开 Vim 让你填提交信息。如果你没用过 Vim，估计连退出都不会，所以不如直接在命令行提供提交信息：

```bash
git commit -m 'commmit info'
```

不推荐不填提交信息，提交信息对人对己都有好处。



# 总结

&emsp;&emsp;是不是很简单！但实际上 Git 的学问可多着呢，但学会上面那些对于大学cpp课程来讲，足矣~