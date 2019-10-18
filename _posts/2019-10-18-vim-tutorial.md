---
layout: post
title:  vim 入门
date:   2019-10-18 16:21:00 +0800
categories: document
tag: [Linux]
music-id: 481853665
---

> vim 从启动到退出

<!-- more -->



![vim键盘图](https://www.runoob.com/wp-content/uploads/2015/10/vi-vim-cheat-sheet-sch.gif "vim键盘图")



# 前言

&emsp;&emsp;虽然编辑文本文件用 nano 足矣，但是写程序的时候总是不那么方便，所以 vim 还是要学一下的~ 在正式学习之前，先说说如何退出vim：\<Esc\> + 输入:wq + \<Enter\>，很多人第一次用 vim 时完全不知道如何退出，所以你去搜索 vim，大部分人都是问这个问题 ~



# 三种模式

&emsp;&emsp;和 Jupyter Notebook 类似，vim 也有多种模式：

1. 基本模式（Command mode）

   刚启动时就进入命令模式，此时可以输入基本命令。

2. 输入模式（Insert mode）

   可以编辑文字

3. 命令模式（Last line mode）

   可以输入比命令模式更多的命令
   
4. 浏览模式（Visual mode）

   可以看（暂时还不知道这是干什么的）



&emsp;&emsp;三种模式的切换方法如下：

![模式的切换方法](https://cs61a.org/articles/assets/vim-modes.png "模式的切换方法")

## 基本模式

&emsp;&emsp;在基本模式下可以做一些基本的操作，比如 移动光标、复制粘贴、搜索替换。

### 移动光标

&emsp;&emsp;除了 ↑↓←→外，没有方向键的键盘也可以用 `h左`、`j下`、`k下`、`l右` 来移动光标。

&emsp;&emsp;除了 PgUp 和 PhDn 外，也可以用 Ctrl+f（front，上一页）、Ctrl+b（below，下一页）来翻页。

&emsp;&emsp;除了 Home 和 End 外，也可以用 0（最前面）、$（最后面）来让光标移动到行的前面或后面。

&emsp;&emsp;如果想一次移动多行，直接输入数字就好。

&emsp;&emsp;如果想移动到第n行，可以用 n+G。

&emsp;&emsp;想移动到最后一行，可以用 G。



### 复制粘贴

&emsp;&emsp;不得不说没了鼠标，复制这活可不好干。vim 只能这样复制：

* 复制光标所在行：yy
* 复制光标到下面n行：nyy
* 复制光标到行首：y0
* 复制光标到行末：y$
* 有一些不常用，就不列了

&emsp;&emsp;至于粘贴，有两种：

* 在光标那行下面粘贴：p（小写）
* 在光标那行上面行粘贴：P（大写）



### 搜索替换

&emsp;&emsp;搜索：

* 向下搜索一个字符串：/+字符串
* 向上搜索一个字符串：?+字符串

&emsp;&emsp;如果你想不断地找下一个的话，你可以输一次上面的命令，然后输入 n，就可以重复前面的搜索动作。如果你想沿前面搜索动作的反方向找，可以输入 N

&emsp;&emsp;至于替换，有一个格式：

* :n1,n2s/word1/word2/gc

&emsp;&emsp;意思是将 n1 行到 n2 行中，所有的 word1 替换为 word2。特殊地，第一行到最后一行表示为：`:1,$s/word1/word2/gc`



### 删除

&emsp;&emsp;除了用 backspace 和 del，还可以用：

* 向前删除 X，向后删除 x
* 删除一整行：dd
* 向下删除 n 行：ndd



## 输入模式

&emsp;&emsp;输入模式和一般的文本编辑器没什么区别，可以输入字符，也可以 ↑↓←→等等。唯一要讲的是，如何从基本模式进入输入模式。

&emsp;&emsp;一般按 i （insert）就是从光标所在处输入；如果你想让你的输入取代后面的字，可以用 r，R（replace），小写是只取代一个字，大写是一直取代。知道这两种方法就行了，记太多会乱。                                                                                                                         



## 命令模式

&emsp;&emsp;其实命令模式和一般模式差不多，在一般模式下输入 : 就会进入命令模式，输入完一条命令后，就会自动跳回一般模式。所以很多时候，教程中都不会单独说命令模式。在此仅简单介绍一些常用命令：

* 储存：`:w`
* 退出：`:q`
* 储存并退出：`:wq`
* 另存为：`:w [filename]`
* 在 bash 中执行 command（比如 ls）：`:! command`



# 最基本的使用流程

&emsp;&emsp;写代码的基本流程是：新建文件 -> 编辑 -> 保存，那么在 vim 中对应的操作就是：

1. 新建一个文件

   ```bash
   vim new.c 
   ```

2. 按 i 进入输入模式
3. 按 Esc 退出输入模式，进入命令模式，按 :wq 保存并退出



# 参考

* [简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)