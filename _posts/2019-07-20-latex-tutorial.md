---
layout: post
#标题配置
title:  LaTeX入门
#时间配置
date:   2019-07-20 16:15:17 +0800
#大类配置
categories: document
#小类配置
tag: LaTeX
#网易云音乐，只能播放无版权保护的
music-id: 465675773
---

* content
{:toc}
> 本文参考了[https://liam.page](https://liam.page/)的博文和油管上的LaTex Tutoial，如有侵权，请联系我。

# LaTeX简介

&emsp;&emsp;LaTeX是一种排版工具，与word不同，LaTeX的排版并不是“所见即所得”，而是依靠编码描述（类似于打代码），只有在最终生成时才可以看到效果。关于LaTeX的历史，我在此不赘述。

&emsp;&emsp;选择LaTeX的理由很简单：公式。Word对数学公式的支持不好，而LaTeX对公式的处理则优雅得多。



# 安装

&emsp;&emsp;为了确保我的伙伴与我使用的环境相同，我还是详细写一下我的安装方法。我使用的是TexLive包，先下载[https://pan.baidu.com/s/1chu4I3DQhOTtVFcES2VVUg](https://pan.baidu.com/s/1chu4I3DQhOTtVFcES2VVUg)（提取码：t9io），下载完后，跟着这里说的做：[https://liam.page/texlive](https://liam.page/texlive)



# 基本

## Hello, world!

&emsp;&emsp;打开TexWorks editor，它的界面和记事本差不多，各个按钮都有中文解释，在此就不多说。

![TexWorks](https://cemclinux1.math.uwaterloo.ca/~math600/wp/wp-content/uploads/2012/07/TeXworks1.png "TexWorks界面")

&emsp;&emsp;在界面中输入如下内容（即图片中的内容）：

~~~latex
%第一个LaTeX文档
\documentclass{article}
\begin{document}
Hello, world!
\end{document}
~~~

&emsp;&emsp;然后按左上的三角形按钮，就会生成pdf并在右边显示。

![Hello, world!](http://cemclinux1.math.uwaterloo.ca/~math600/wp/wp-content/uploads/2012/07/image02.png "Hello, world!效果")

&emsp;&emsp;看起来也不是很难嘛（和编程差不多）。下面说说各语句的意思：

* 以反斜杠“ \ ”开头的那串东西是LaTeX的**控制序列**（也称命令/标记），用于控制输出效果。

  * 控制序列中间不能有空格；

  * 控制序列的必要参数用 { } 包围，可选参数用 [ ] 包围；

  * LaTeX对控制序列的大小写敏感；

* 注释前面用%，%后面的文字会被忽略
  * 要正常输入%，需要输入\%，这个和python的转义字符相同
* `\documentclass[arg1,arg2,...]{arg}` 用于指定该文档的布局。
  * 必要参数用于指定文档类型，可选：
    * `article`：用于学术期刊、编程文档等单篇文章
    * `proc`：基于article的一种类型
    * `minimal`：最小的类型，只规定了页面大小和字体
    * `book`：用于真实的书
    * `slide`：幻灯片
    * `memoir`：基于book的一种类型
    * `letter`：用于信件
    * `beamer`：用于presentations
  * 非必要参数之间用 “ , ” 隔开，可有：
    * 纸张大小：`letterpaper`,``legalpaper`，`a4paper`，`a5paper`，`b5paper`
    * 字体：`12pt`（数字可变）
    * 将每页分为两边显示：`twocolumn`
* `\documentclass{}` 到 `\begin{document}` 之间的区域为**导言区**，用于对整篇文档进行设置。上面并没导言区，但我们很快就要用到。
* 只有 `\begin{document}` 和 `\end{document}` 之间的内容才会输出到文档中，并且`\end{document}`后的内容和控制序列都是无效的。（即`\end{document`）是文档的结束标志）
* 还有其他`\begin{...}`，`\end{...}`，这类东西我们称之为**环境**



## 输出中文

&emsp;&emsp;TeX本身不支持中文，而XeLaTeX支持Unicode，所以只需加载中文宏包即可。**宏包**指的是：

> 所谓宏包，就是一系列控制序列的合集。这些控制序列太常用，以至于人们会觉得每次将他们写在导言区太过繁琐，于是将他们打包放在同一个文件中，成为所谓的宏包（台湾方面称之为「巨集套件」）。`\usepackage{}` 可以用来调用宏包。

&emsp;&emsp;重新输入下面的内容，以UTF-8编码保存，使用XeLaTeX编译：

```latex
\documentclass[UTF8]{ctexart}
\begin{document}
你好，world!
\end{document}
```

&emsp;&emsp;在这个例子中，我们用 `ctexart` 代替了 `article` ，并增加了 `UTF8` 的参数。注意：`UTF8` 参数对于 `ctexart` 是必不可少的，否则将出现乱码。

&emsp;&emsp;你可能说，F**K，都没用到 `\usepackage{}` ，都没用到宏包，骗人哦~ 上面的只是简化的写法，复杂的写法如下：

```LaTeX
\documentclass{article}
\usepackage{xeCJK} %调用 xeCJK 宏包
\setCJKmainfont{SimSun} %设置 CJK 主字体为 SimSun （宋体）
\begin{document}
你好，world!
\end{document}
```

&emsp;&emsp;`\setCJKmainfont{}` 用于指定中文字体，你可以打开 C:\Windows\Fonts 查看当前系统可用的中文字体，并填入中文或对应英文（右键属性查看）

>注：如果想要查看宏包的具体使用方法，打开 Tex Live command-line，输入 `texdoc 宏包名` 即可查看宏包的文档



## 组织文章内容

### 作者、标题、日期

```latex
\documentclass[UTF8]{ctexart}
\title{你好，world!}%标题
\author{Todd}%作者
\date{\today}%日期
\begin{document}
\maketitle%显示作者、标题、日期
你好，world!
\end{document}
```

&emsp;&emsp;上面的控制序列都很容易懂，`\maketitle` 可以将标题、作者、日期按一定格式显示出来，至于如何修改这个格式，需要 titling 宏包，在此不作赘述。

&emsp;&emsp;值得一提的是，如果不写日期，只写标题和作者，则 \maketitle 会显示编译的日期，要想不显示日期，只需将 \date{} 括号中置空即可。



### 章节和段落

&emsp;&emsp;在 article 和 ctexart 中，有五个控制序列来调整文章结构，从大到小分别为：

* `\section{...}`：比如 “1 行列式”
* `\subsection{...}`：比如 “1.1 n阶行列式的定义”
* `\subsubsection{...}`：比如"1.1.1 二阶行列式与三阶行列式"
* `\paragraph{...}`：段落
* `\subparagraph{...}`：向右缩进的小段落

&emsp;&emsp;输入下面内容实践一下：

```LaTeX
\documentclass[UTF8]{ctexart}
\title{线性代数与几何}
\author{清华大学出版社}
\date{\today}
\begin{document}
\maketitle
\section{行列式}
行列式是重要的数学工具。
\subsection{n阶行列式的定义} 
在本节中，我们将先对二阶和三阶行列式的定义以及如何利用它们求解二元和三元线性方程组，作一简单的回顾，然后介绍排列的概念及其基本性质，最后给出n阶行列式的定义。
\subsubsection{二阶行列式与三阶行列式}
\paragraph{二元线性方程组}消元可得...
\subparagraph{二阶行列式} 为了便于记忆这些解的公式，我们引入二阶行列式...
\subsection{排列}
\paragraph{n阶排列} 是由1,2,3,...,n组成的有序数组。
\end{document}
```

&emsp;&emsp;在 report 和 ctexrep 中，还有 `\chapter{...}`；在 book和ctexbook中，还有 `\part{...}`。



### 目录

&emsp;&emsp;在 `\maketitle` 下面插入 `\tableofcontents`，**编译两次**，就能看到带页码的目录。这种目录只占据一小块区域。

&emsp;&emsp;如果在`\maketitle` 上面插入 `\tableofcontents`，则目录占据一整页。



## 字体

&emsp;&emsp;下面是一些常用的字体设置：

* 英语加粗/中文黑体：`\textbf{...}`（只对括号内有效）；`\mdseries`（对后面都有效）（下同）
* 英语斜体/中文楷体：`\textit{...}`；`\itshape`
* 字号：（相对于设定的normalsize）从小到大分别为：`\tiny{...}`、`\scriptsize{...}`、`\footnotesize{...}`、`\small{...}`、`\nolmalsize{...}`、`\large{...}`、`\Large{...}`、`\LARGE{...}`、`\huge{...}`、`\Huge{...}`
* 下划线：`\underline{...}`
* 其他线：`\usepackage{ulem}`
  * `\uline{...}` 下划线
  * `\uuline{...}` 双下划线
  * `\uwave{...}`  波浪线
  * `\sout{...}` 删除线
  * `\xout{...}` 斜删除线



&emsp;&emsp;只用于英文的设置：

* 罗马字体/花体：`\textrm{...}` ；`\rmfamily` 
* 无衬线字体：`\textsf{...}`；`\sffamily`
* 打字机字体/等宽字体：`\texttt{...}`；`ttfamily`



&emsp;&emsp;只用于中文的字体设置：

* 字体：`\songti{...}`、`\heiti{...}`、`\fangsong{...}`、`\kaishu{...}`
* 字号：`\zihao{0}` 初号、`\zihao{1}` 一号、`zihao{-1}` 小一，以此类推



&emsp;&emsp;如果某些字体设置组合比较常用，可以定义一个自己的命令比如：`\newcommand{\myfont}{\textit{\textbf{My Font}}}`