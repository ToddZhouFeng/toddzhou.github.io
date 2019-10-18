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

   可以看



&emsp;&emsp;三种模式的切换方法如下：

![模式的切换方法](https://cs61a.org/articles/assets/vim-modes.png "模式的切换方法")

## 基本模式

&emsp;&emsp;在基本模式下可以做一些基本的操作，比如 移动光标、复制粘贴、搜索替换。

### 移动光标

&emsp;&emsp;除了 ↑↓←→外，没有方向键的键盘也可以用 `h左`、`j下`、`k下`、`l右` 来移动光标。

&emsp;&emsp;除了 PgUp 和 PhDn 外，也可以用 Ctrl+f（上一页）、Ctrl+b（下一页）来翻页。

&emsp;&emsp;除了 Home 和 End 外，也可以用 0（最前面）、$（最后面）来让光标移动到行的前面或后面。

&emsp;&emsp;如果想一次移动多行，直接输入数字就好。

&emsp;&emsp;如果想移动到第n行，可以用 n+G。

&emsp;&emsp;想移动到最后一行，可以用 G。





## 输入模式

&emsp;&emsp;输入模式和一般的文本编辑器没什么区别，可以输入字符，也可以 ↑↓←→等等









# 最基本的使用方法

&emsp;&emsp;写代码的基本流程是：新建文件 -> 编辑 -> 保存，那么在 vim 中对应的操作就是：

1. 新建一个文件

   ```bash
   vim new.c 
   ```

2. 按 i 进入输入模式
3. 按 Esc 退出输入模式，进入命令模式，按 :wq 保存并退出



# 参考

* [简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)