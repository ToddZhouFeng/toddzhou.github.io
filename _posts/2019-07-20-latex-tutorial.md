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
music-id: 465675773
---



> 本文参考了[https://liam.page](https://liam.page/)的博文和油管上的LaTex Tutoial，如有侵权，请联系我。

# LaTeX简介

&emsp;&emsp;LaTeX是一种排版工具，与word不同，LaTeX的排版并不是“所见即所得”，而是依靠编码描述（类似于打代码），只有在最终生成时才可以看到效果。关于LaTeX的历史，我在此不赘述。

&emsp;&emsp;选择LaTeX的理由很简单：公式。Word对数学公式的支持不好，而LaTeX对公式的处理则优雅得多。



# 安装

&emsp;&emsp;为了确保我的伙伴与我使用的环境相同，我还是详细写一下我的安装方法。我使用的是 TexLive 包，先下载[https://pan.baidu.com/s/1chu4I3DQhOTtVFcES2VVUg](https://pan.baidu.com/s/1chu4I3DQhOTtVFcES2VVUg)（提取码：t9io），下载完后，跟着这里说的做：[https://liam.page/texlive](https://liam.page/texlive)

&emsp;&emsp;关于为什么用 TexLive，首先它支持Windows，Linux，Mac OS X，其次它支持中文，再其次它现在依然在维护，最后它安装方便。



# 基本

## Hello, world!

&emsp;&emsp;打开 TexLive 自带的编辑软件 TexWorks editor，它的界面和记事本差不多，各个按钮都有中文解释，在此就不多说。

![TexWorks](https://cemclinux1.math.uwaterloo.ca/~math600/wp/wp-content/uploads/2012/07/TeXworks1.png "TexWorks界面")

&emsp;&emsp;在界面中输入如下内容（即图片中的内容）：

~~~latex
%第一个LaTeX文档
\documentclass{article}
\begin{document}

Hello, world!

\end{document}
~~~

&emsp;&emsp;然后按左上的三角形按钮，就会生成 pdf 并在右边显示。

![Hello, world!](http://cemclinux1.math.uwaterloo.ca/~math600/wp/wp-content/uploads/2012/07/image02.png "Hello, world!效果")

&emsp;&emsp;看起来也不是很难嘛（和编程差不多）。下面说说各语句的意思：

* 以反斜杠“ \ ”开头的那串东西是 LaTeX 的**控制序列**（也称命令/标记），用于控制输出效果。

  * 控制序列中间不能有空格；

  * 控制序列的必要参数用 { } 包围，可选参数用 [ ] 包围；

  * LaTeX对控制序列的大小写敏感；

* 注释前面用%，%后面的文字会被忽略
  
  * 要正常输入%，需要输入\%，这个和 python 的转义字符相同
  
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

* 值得注意的是，在 LaTeX 中，单个回车符相当于空格，要换行必须用两个回车符，即

  ```latex
  \begin{document}
  Hello, world!
  from Todd
  \end{document}
  %显示
  %Hello, world! from Todd
  ```

  ```latex
  \begin{document}
  Hello, world!
  
  from Todd
  \end{document}
  %显示
  %Hello, world!
  %from Todd
  ```

  



## 输出中文

&emsp;&emsp;TeX 本身不支持中文，而 XeLaTeX 支持 Unicode，所以只需加载中文宏包即可。**宏包**指的是：

> 所谓宏包，就是一系列控制序列的合集。这些控制序列太常用，以至于人们会觉得每次将他们写在导言区太过繁琐，于是将他们打包放在同一个文件中，成为所谓的宏包（台湾方面称之为「巨集套件」）。`\usepackage{}` 可以用来调用宏包。

&emsp;&emsp;重新输入下面的内容，以 UTF-8 编码保存，使用 XeLaTeX 编译：

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



### 章节层次

&emsp;&emsp;在 article 和 ctexart 中，有五个控制序列来调整文章结构，从浅到深分别为（后面数字为深度）：

* `\section{...}`：比如 “1 行列式”——1
* `\subsection{...}`：比如 “1.1 n阶行列式的定义”——2
* `\subsubsection{...}`：比如"1.1.1 二阶行列式与三阶行列式"——3
* `\paragraph{...}`：段落——4
* `\subparagraph{...}`：向右缩进的小段落——5

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

&emsp;&emsp;在 report 和 ctexrep 中，还有 `\chapter{...}`—— 0；在 book和ctexbook中，还有 `\part{...}`—— -1。



### 目录

&emsp;&emsp;在 `\maketitle` 下面插入 `\tableofcontents`，**编译两次**，就能看到带页码的目录。这种目录只占据一小块区域。

&emsp;&emsp;如果在`\maketitle` 上面插入 `\tableofcontents`，则目录占据一整页。

&emsp;&emsp;如果要指定显示的深度，可以用：`\setcounter{tocdepth}{2}`，只显示深度小于等于2的章节层次。



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



&emsp;&emsp;如果某些字体设置组合比较常用，可以定义一个自己的命令比如：`\newcommand{\myfont}[1]{\textit{\textbf{#1}}}`，然后可以直接用`\myfont{...}`。关于`\newcommand{}[]{}`的更多用法，请看[详解newcommand的参数和默认值](http://softlab.sdut.edu.cn/blog/subaochen/2017/07/详解newcommand的参数和默认值/)。



## 行距

&emsp;&emsp;默认使用单倍行距，若要修改，可以用：

```latex
\usepackage{setspace}
\singlespacing %单倍行距
\onehalfspacing %1.5倍行距
\doublespacing %双倍行距
\setstretch{1.25} %任意倍行距
```



## 引用

&emsp;&emsp;在 LaTeX 中，可以在对象（章、节、图、表）后面设置标签，然后在其他位置引用标签来引用对象的页码或编号。

```LaTeX
\documentclass[UTF8]{ctexart}
\begin{document}
一个标签\label{marker}
...
第\pageref{marker}页第\ref{marker}节
\end{document}
```

&emsp;&emsp;文档中新增标签后，要编译两次才能显示正确的结果。





# 数学公式

&emsp;&emsp;数学公式可是LaTeX最有价值的部分之一。首先，我们需要在导言区加载 amsmath 宏包：

```latex
\usepackage{amsmath}
```



## 数学模式

&emsp;&emsp;LaTeX 的数学模式有两种：行内模式 ( inline ) 和 行内模式 ( display )。顾名思义，就是一个在正文中插入数学公式，另一个单独成行，并自动居中。用法如下：

```latex
%行内模式
\( ... \)

%无编号的行间模式
\[ ... \]

%有编号的行间公式
\begin{equation}
...
\end{equation}
```

&emsp;&emsp;还有其他控制序列用于插入公式，但太麻烦了，不推荐，仅列举作为参考：

```latex
%行内公式
$ ... $
\begin{math} ... \end{math}

%无编号的行间公式
$$ ... $$
\begin{displaymath} ... \end{displaymath}
\begin{equation*} ... \end{equation*}
```



## 国标以及对应的输入方法

&emsp;&emsp;国标“ GB 3102.11 物理科学和技术中使用的数学符号 ”中定义了一系列的符号和输入方法。下面选取其中比较重要的部分。



### 变量、函数、点、线、弧

&emsp;&emsp;变量（x, y）、函数（f, g）、点（A）、线段（AB）、弧（CD）用**斜体**表示，在 LaTeX 中输入的字母默认就是斜体。



### 有定义的函数、数学常数、算子

&emsp;&emsp;有定义的函数（sin, log）、数学常数（pi, e）、算子（d, div）用**正体**表示，在 LaTeX 中的输入方法如下：

```latex
\rm{e}=2.7182818
%或者
\mathrm{e}=2.7182818
```

&emsp;&emsp;对于常用有定义的函数，LaTeX中有对应的命令，如：

```latex
\ln  \lg  \log  \sin  \arcsin  \sinh  \max
```

$$
\ln \; \lg \; \log \; \sin \; \arcsin \; \sinh \; \max
$$



### 括号及间隙

&emsp;&emsp;函数的自变量写在后面的括号中，函数和括号之间不留空格；如果自变量不含 +, -, ×, ÷ 等运算符，则可以省略括号，并且函数和自变量之前留一空格；特别的，如果有可能混淆，如 cos x + y，应该加括号，写成：cos(x) + y 或 (cos x) + y



### 几何、集合、逻辑、运算等符号

&emsp;&emsp;参考 [Markdown LaTeX 数学符号速查表](https://www.rdtoc.com/tutorial/markdown-latex-tutorial.html) ，有一些常用符号在下面列出。



## 基本元素

### 希腊字母

<div class="table-box"><table style="color:rgb(54,46,43); font-family:Arial; font-size:14px; line-height:26px; text-align:left">
<tbody>
<tr>
<th colspan="4">
<center>正体希腊字母</center>
</th>
</tr>
<tr>
<th>特征</th>
<th>语法</th>
<th>效果</th>
<th>注释/外部链接</th>
</tr>
<tr>
<th rowspan="3">
<center>大写字母</center>
</th>
<td><code>\Alpha \Beta \Gamma \Delta \Epsilon \Zeta \Eta\Theta</code></td>
<td><img alt="\Alpha\Beta\Gamma\Delta\Epsilon\Zeta\Eta\Theta\!" src="http://upload.wikimedia.org/wikipedia/zh/math/a/7/a/a7a8e6bbde24e99f9dab00c840f9483d.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<th>
<center><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%91" rel="nofollow" title="Α" style="color:rgb(106,57,6); text-decoration:none">Α</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%92" rel="nofollow" title="Β" style="color:rgb(106,57,6); text-decoration:none">Β</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%93" rel="nofollow" title="Γ" style="color:rgb(106,57,6); text-decoration:none">Γ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%94" rel="nofollow" title="Δ" style="color:rgb(106,57,6); text-decoration:none">Δ</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%95" rel="nofollow" title="Ε" style="color:rgb(106,57,6); text-decoration:none">Ε</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%96" rel="nofollow" title="Ζ" style="color:rgb(106,57,6); text-decoration:none">Ζ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%97" rel="nofollow" title="Η" style="color:rgb(106,57,6); text-decoration:none">Η</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%98" rel="nofollow" title="Θ" style="color:rgb(106,57,6); text-decoration:none">Θ</a></center>
</th>
</tr>
<tr>
<td><code>\Iota \Kappa \Lambda \Mu \Nu \Xi \Omicron \Pi</code></td>
<td><img alt="\Iota\Kappa\Lambda\Mu\Nu\Xi\Omicron\Pi\!" src="http://upload.wikimedia.org/wikipedia/zh/math/0/e/0/0e022245aac65d3fe5eaf14065761066.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<th>
<center><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%99" rel="nofollow" title="Ι" style="color:rgb(106,57,6); text-decoration:none">Ι</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%9A" rel="nofollow" title="Κ" style="color:rgb(106,57,6); text-decoration:none">Κ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%9B" rel="nofollow" title="Λ" style="color:rgb(106,57,6); text-decoration:none">Λ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%9C" rel="nofollow" title="Μ" style="color:rgb(106,57,6); text-decoration:none">Μ</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%9D" rel="nofollow" title="Ν" style="color:rgb(106,57,6); text-decoration:none">Ν</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%9E" rel="nofollow" title="Ξ" style="color:rgb(106,57,6); text-decoration:none">Ξ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%9F" rel="nofollow" title="Ο" style="color:rgb(106,57,6); text-decoration:none">Ο</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A0" rel="nofollow" title="Π" style="color:rgb(106,57,6); text-decoration:none">Π</a></center>
</th>
</tr>
<tr>
<td><code>\Rho \Sigma \Tau \Upsilon \Phi \Chi \Psi\Omega</code></td>
<td><img alt="\Rho\Sigma\Tau\Upsilon\Phi\Chi\Psi\Omega\!" src="http://upload.wikimedia.org/wikipedia/zh/math/e/f/c/efc3244ff48644b6134ee45563b49b45.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<th>
<center><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A1" rel="nofollow" title="Ρ" style="color:rgb(106,57,6); text-decoration:none">Ρ</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A3" rel="nofollow" title="Σ" style="color:rgb(106,57,6); text-decoration:none">Σ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A4" rel="nofollow" title="Τ" style="color:rgb(106,57,6); text-decoration:none">Τ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A5" rel="nofollow" title="Υ" style="color:rgb(106,57,6); text-decoration:none">Υ</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A6" rel="nofollow" title="Φ" style="color:rgb(106,57,6); text-decoration:none">Φ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A7" rel="nofollow" title="Χ" style="color:rgb(106,57,6); text-decoration:none">Χ</a>&nbsp;<a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A8" rel="nofollow" title="Ψ" style="color:rgb(106,57,6); text-decoration:none">Ψ</a><a target="_blank" href="http://zh.wikipedia.org/wiki/%CE%A9" rel="nofollow" title="Ω" style="color:rgb(106,57,6); text-decoration:none">Ω</a></center>
</th>
</tr>
<tr>
<th rowspan="3">
<center>小写字母</center>
</th>
<td><code>\alpha \beta \gamma \delta \epsilon \zeta \eta\theta</code></td>
<td><img alt="\alpha\beta\gamma\delta\epsilon\zeta\eta\theta\!" src="http://upload.wikimedia.org/wikipedia/zh/math/9/f/e/9fef94989f0aefed4c953823bd945e89.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\iota \kappa\varkappa \lambda \mu \nu \xi \omicron\pi</code></td>
<td><img alt="\iota\kappa\varkappa\lambda\mu\nu\xi\omicron\pi\!" src="http://upload.wikimedia.org/wikipedia/zh/math/2/1/f/21fef848efb59d5bf6168bfb9a8755ab.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\rho \sigma \tau \upsilon \phi \chi \psi\omega</code></td>
<td><img alt="\rho\sigma\tau\upsilon\phi\chi\psi\omega\!" src="http://upload.wikimedia.org/wikipedia/zh/math/1/1/3/1136b70309d44ecdbacbba66bf42ceb8.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<th rowspan="7">
<center>异体字母</center>
</th>
<td><code>\Epsilon\epsilon\varepsilon</code></td>
<td><img alt="\Epsilon\epsilon\varepsilon" src="http://upload.wikimedia.org/wikipedia/zh/math/9/9/c/99c715d6c6eb8fb0c7788b0496c18fcf.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\Theta\theta\vartheta</code></td>
<td><img alt="\Theta\theta\vartheta" src="http://upload.wikimedia.org/wikipedia/zh/math/c/8/5/c85d2107efa86dfe8e8954e8104941c6.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\Kappa\kappa\varkappa</code></td>
<td><img alt="\Kappa\kappa\varkappa" src="http://upload.wikimedia.org/wikipedia/zh/math/7/b/8/7b8b29c367a46e3d1403cb76cbbbf028.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\Pi\pi\varpi</code></td>
<td><img alt="\Pi\pi\varpi" src="http://upload.wikimedia.org/wikipedia/zh/math/2/1/5/215d2b61e8b6fa92a374ea7f6fd0adf6.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\Rho\rho\varrho</code></td>
<td><img alt="\Rho\rho\varrho" src="http://upload.wikimedia.org/wikipedia/zh/math/3/1/a/31accc86698f4bc68e6a7fc57d5aba5e.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\Sigma\sigma\varsigma</code></td>
<td><img alt="\Sigma\sigma\varsigma" src="http://upload.wikimedia.org/wikipedia/zh/math/6/3/3/633e6a1a75704bfc8ccb0ad3aafa22d1.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<td><code>\Phi\phi\varphi</code></td>
<td><img alt="\Phi\phi\varphi\," src="http://upload.wikimedia.org/wikipedia/zh/math/0/f/f/0ff9115861d9a54f1529074db5fef99f.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<td>&nbsp;</td>
</tr>
<tr>
<th>
<center>已停用字母</center>
</th>
<td><code>\digamma</code></td>
<td><img alt="\digamma" src="http://upload.wikimedia.org/wikipedia/zh/math/2/f/0/2f057b6e514c8ca2d9cf9a3e549f8865.png" title="Texlive:&nbsp;<wbr>latex数学符号表(2)" style="border:none; max-width:100%"></td>
<th>
<center><a target="_blank" href="http://zh.wikipedia.org/wiki/%CF%9C" rel="nofollow" title="Ϝ" style="color:rgb(106,57,6); text-decoration:none">Ϝ</a><sup><a target="_blank" href="http://zh.wikipedia.org/wiki/Help:MATH#cite_note-waw-0" rel="nofollow" style="color:rgb(106,57,6); text-decoration:none">[1]</a></sup></center>
</th>
</tr>
</tbody>
</table></div>





### 上下标

&emsp;&emsp;上标用 `^` ，下标用 `_` 。它们只作用于后一个字符，如果要作用于多个字符，则要将字符用 `{ }` 括起来。比如：

```latex
{}_1^2 x _{ij}^2
```

$$
{}_1^2 x _{ij}^2
$$



### 分数/分式

&emsp;&emsp;`\frac{分子}{分母}` 用于输入分数/分式，其字号大小取决于模式，在行内模式下会小一点，在行间模式下会大一点，我们也可以用 `\dfrac{}{}` （行内）或 `\tfarc{}{}` （行间）指定其大小。如：

```latex
\frac{1}{2}  \dfrac{1}{2}  \tfrac{1}{2}
```

$$
\frac{1}{2} \, \dfrac{1}{2} \, \tfrac{1}{2}
$$



### 根号/根式

&emsp;&emsp;`\sqrt[次数]{开根数}` 用于输入根号/根式，次数不写的话就是平方根，如：

```latex
\sqrt{3}  \sqrt[3]{3}
```

$$
\sqrt{3} \, \sqrt[3]{3}
$$



### 运算符

&emsp;&emsp;键盘上有的运算符可以直接输入，没有的需要特殊命令：

```latex
\pm  \times  \div  \cdot  \cap  \cup  \geq  \leq  \neq  \approx  \equiv
```

$$
\pm \; \times \; \div \; \cdot \; \cap \; \cup \; \geq \; \leq \; \neq \; \approx \; \equiv
$$

```latex
\sum_{k=1}^N k^2 
\prod_{i=1}^N x_i
\lim_{n \to \infty}x_n
\int_{-N}^{N} e^x\, \mathrm{d}x
```

$$
\sum_{k=1}^N k^2\\
\prod_{i=1}^N x_i\\
\lim_{n \to \infty}x_n\\
\int_{-N}^{N} e^x\, \mathrm{d}x
$$



### 括号

&emsp;&emsp;由于 LaTeX 中 `{ }`  有特殊用途，故要输入必须在前面加`\`，如：

```latex
\{ x \}
```

$$
\{ x \}
$$

&emsp;&emsp;括号的写法如下：

```LaTeX
\Biggl( \bigg( \Big( \big( (x) \big) \Big) \bigg) \Bigg)
```

$$
\Biggl( \bigg( \Big( \big( (x) \big) \Big) \bigg) \Bigg)
$$

```latex
\Bigg[ \bigg[ \Big[ \big[ [x] \big] \Big] \bigg] \Bigg]
```

$$
\Bigg[ \bigg[ \Big[ \big[ [x] \big] \Big] \bigg] \Bigg]
$$

```latex
\Bigg\{ \bigg\{ \Big\{ \big\{ \{ x \} \big\} \Big\} \bigg\} \Bigg\}
```

$$
\Bigg\{ \bigg\{ \Big\{ \big\{ \{ x \} \big\} \Big\} \bigg\} \Bigg\}
$$

```latex
\Bigg| \bigg|  \Big| \big| |x| \big| \Big| \bigg| \Bigg|
%或者
\Bigg \lvert \bigg \lvert \Big \lvert \big \lvert \lvert x \rvert \big \rvert \Big \rvert \bigg \rvert \Bigg \rvert
```

$$
\Bigg \lvert \bigg \lvert \Big \lvert \big \lvert \lvert x \rvert \big \rvert \Big \rvert \bigg \rvert \Bigg \rvert
$$

```latex
\Bigg \lVert \bigg \lVert \Big \lVert \big \lVert \lVert x \rVert \big \rVert \Big \rVert \bigg \rVert \Bigg \rVert
```

$$
\Bigg \lVert \bigg \lVert \Big \lVert \big \lVert \lVert x \rVert \big \rVert \Big \rVert \bigg \rVert \Bigg \rVert
$$



### 省略号

```latex
\dots  \cdots  \vdots  \ddots
```

$$
\dots \; \cdots \; \vdots \; \ddots
$$



### 空格

&emsp;&emsp;LaTeX中的空格只起到分割命令的作用，要输入空格必须使用如下命令：

```LaTeX
\!  \,  \:  \;  \quad  \qquad
```

$$
\text{正常：}|| \\
|\!|  \quad  |\,|  \quad  |\:|  \quad  |\;|  \quad  |\quad|  \quad  |\qquad|
$$



## 块状元素

### 矩阵

&emsp;&emsp;使用 pmatrix、bmatrix、Bmatrix、vmatrix、Vmatrix 环境来写不同括号的矩阵，矩阵中用 `&` 来分隔不同列，用 `\\` 分隔不同行。

```latex
\begin{pmatrix} a&b\\c&d \end{pmatrix}
\begin{bmatrix} a&b\\c&d \end{bmatrix}
\begin{Bmatrix} a&b\\c&d \end{Bmatrix}
\begin{vmatrix} a&b\\c&d \end{vmatrix}
\begin{Vmatrix} a&b\\c&d \end{Vmatrix}
```

$$
\begin{pmatrix} a&b\\c&d \end{pmatrix} \quad
\begin{bmatrix} a&b\\c&d \end{bmatrix} \quad
\begin{Bmatrix} a&b\\c&d \end{Bmatrix} \quad
\begin{vmatrix} a&b\\c&d \end{vmatrix} \quad
\begin{Vmatrix} a&b\\c&d \end{Vmatrix} \quad
$$

&emsp;&emsp;行内的小矩阵可以用 smallmatrix：

```latex
$ ( \begin{smallmatrix} a&b\\c&d \end{smallmatrix} ) $
```

$$
( \begin{smallmatrix} a&b\\c&d \end{smallmatrix} )
$$

&emsp;&emsp;对于复杂的矩阵，可以用 array ，它可以设置对齐模式：外部对齐：t 居顶、c 居中（默认）、b 居底；列对齐：l 居左、c 居中、r 居右。矩阵中用 `&` 来分隔不同列，用 `\\` 分隔不同行。

```latex
\begin{array}[外部对齐]{列对齐}
...
\end{array}
```

```latex
\begin{array}{ccc}
x_{11} & x_{12} & \dots \\
x_{21} & x_{22} & \dots \\
\vdots & \vdots & \ddots
\end{array}
```

$$
\begin{array}{ccc}
x_{11} & x_{12} & \dots \\
x_{21} & x_{22} & \dots \\
\vdots & \vdots & \ddots
\end{array}
$$



### 长公式

&emsp;&emsp;无需对齐的长公式用 multline 环境，要对齐的长公式用 split 环境。用 `\\` 分行，用 `&` 对齐。若要不带编号，用 multline*

```latex
\begin{multline}
x = a+b+c+\\
  d+e+f
\end{multline}
```

$$
\begin{multline}
x = a+b+c+\\
d+e+f
\end{multline}
$$

```latex
\begin{split}
x = & a+b+c+\\
    & d+e+f
\end{split}
```

$$
\begin{split}
x = & a+b+c+\\
    & d+e+f
\end{split}
$$



### 公式组

&emsp;&emsp;不需要对齐的公式组用 gather 环境，需要对齐的公式组用 align 环境。用 `\\` 分行，用 `&` 对齐。若要不带编号，用 gather\*，align\*

```latex
\begin{gather}
a = b+c+d\\
x = y+z
\end{gather}
```

$$
\begin{gather}
a = b+c+d\\
x = y+z
\end{gather}
$$

```latex
\begin{align}
a &= b+c+d\\
x &= y+z
\end{align}
```

$$
\begin{align}
a &= b+c+d\\
x &= y+z
\end{align}
$$



### 分支公式

&emsp;&emsp;分段函数等分支公式可以用 cases 环境。

```latex
\rm{sgn} \; x=
\begin{cases}
0 ,\quad x\leq 0\\
1 ,\quad x>0
\end{cases}
```

$$
\rm{sgn} \; x=
\begin{cases}
0 ,\quad x\leq 0\\
1 ,\quad x>0
\end{cases}
$$





# 一些有用的宏集

## blindtext 测试文字

&emsp;&emsp; `\usepackage{blindtext}` 用法如下

```latex
\document[UTF8]{ctexart}
\usepackage{blindtext}

\begin{document}

\blindtext[2] %生成一段文字，2表示段的长度

\end{document}
```



---

# 参考

维基百科：数学公式：[http://zh.wikipedia.org/wiki/Help:MATH](http://zh.wikipedia.org/wiki/Help:MATH)

一份其实很短的LaTeX入门文档：[https://liam.page/2014/09/08/latex-introduction/](https://liam.page/2014/09/08/latex-introduction/)

LaTeX排版系统简介：[http://www.dralpha.com/zh/tech/Inotes2.pdf](http://www.dralpha.com/zh/tech/Inotes2.pdf)