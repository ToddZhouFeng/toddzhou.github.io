---
layout: post
#标题配置
title:  Jupyter Notebook入门
#时间配置
date:   2019-07-22 23:59:00 +0800
#大类配置
categories: document
#小类配置
tag: Jupyter Notebook
#网易云音乐，只能播放无版权保护的
music-id: 465675773
---

> 大致介绍一下 Jupyter Notebook 怎么用。

<!-- more -->


# 简介

> The Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more.

&emsp;&emsp;官网的介绍巴拉巴拉一大串，其实 Jupyter Notebook （后面简称Jupyter）就是一个 Notebook 只不过这个 Notebook 比较厉害，它不但可以写字，还可以运行代码，还可以加图片、表格，所以非常适合用来学习代码或者编写文档。

&emsp;&emsp;[官网](https://jupyter.org/)提供了一个免费在线试用平台，不过由于使用的人较多，所以能不能登上全凭运气。如果能，可以看到它的界面是这样的：

!["Jupyter Notebook的界面"](http://geospatialtraining.com/wp-content/uploads/2017/08/Jupyter-Notebook-for-ArcGIS-Users.jpg "Jupyter Notebook的界面")



# 安装

&emsp;&emsp;由于我有树莓派，再加之我比较喜欢保持电脑清爽，所以我将树莓派作为一个服务器，再通过电脑访问。你可以视自己的情况决定。



## 树莓派

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
改为（*表示任意ip）
c.NotebookApp.ip = '*'
```

```shell
#c.NotebookApp.open_browser = True
改为（启动时不打开浏览器）
c.NotebookApp.open_browser = False
```

```shell
#c.NotebookApp.port = 8888
改为（在8888端口）
c.NotebookApp.port = 8888
```

```shell
#c.NotebookApp.notebook_dir = ''
改为（初始目录为/home/pi）
c.NotebookApp.notebook_dir = '/home/pi'
```

&emsp;&emsp;配置完成后修改密码：

```shell
jupyter notebook password
```

&emsp;&emsp;然后就可以使用Jupyter Notebook了，方法是在树莓派上输入`jupyter notebook`，然后在电脑上的浏览器中打开`树莓派的ip地址:8888`



## Windows

&emsp;&emsp;如果安装了 Anaconda ，则会自动安装 Jupyter Notebook。也可以用`pip3 install jupyter`（要用最新的 pip）。如果你连 Python 都没安装，那就搜必应吧。

## MacOS

&emsp;&emsp;希望有人能资助我一台Macbook Pro让我研究一下。



# 上手

## 启动与退出

&emsp;&emsp;在命令行输入 `jupyter notebook`，或以管理员模式运行Jupyter Notebook程序，即可启动。然后就可以在浏览器上打开Jupyter.

&emsp;&emsp;Jupyter 默认启动后自动打开浏览器，并只能在本机上的8888端口访问。如果要修改设置，请看[Jupyter Configuration](https://jupyter.readthedocs.io/en/latest/projects/config.html#command-line-options-for-configuration)，或自行查必应。

&emsp;&emsp;启动后，双击 .ipynb 或 .md 后缀的文件即可查看笔记。如果要关闭笔记，不能只是关闭网页，记得要点击 Files - close and halt ，不然笔记的内核会一直运行

&emsp;&emsp;如果要退出，可以在命令行连按两下 *Ctrl + c*，或者按Jupyter浏览器页面右上角的 Quit.



## 界面

&emsp;&emsp;进入 Jupyter 后，首先是 `File` 界面，就是文件管理器；上方可以切换到 `Running`，查看正在运行的 Jupyter 笔记；上方还有 `Cluster` ，这个不用管。

&emsp;&emsp;点击右边，有一个 `New`，用于新建笔记/文件夹；`Upload` 用于从电脑上传文件（顺便提一句，文件后缀为 .ipynb）。

&emsp;&emsp;我们新建一个笔记：点击 `new`，选择 `python3`，浏览器就会跳到一个新界面。这对于英语好的同学，可以点击菜单栏的 *Help - User Interface Tour*，系统会介绍这个界面。这个界面分为几部分：

* 左上角是笔记名，单击可以更改。

* 下面一排是菜单栏，菜单栏的右边显示的是 当前的状态 | 内核 。编辑状态在编辑时会出现 :pencil2: ，可以按 *Esc* 退出编辑模式并进入命令模式；内核右边的小圆圈显示的是内核是否在运行程序，如果想要终止程序，可点击中间那个 :stop_button: 

* 再下一行就是一些按钮，你可以把鼠标放上去，看它们的功能

* 最后就是笔记块，块分成三类：代码块、markdown块 和 raw块。编辑模式下，笔记块会变绿；命令模式下，笔记块变蓝。

  !["编辑模式"](https://jupyter-notebook.readthedocs.io/en/latest/_images/edit_mode.png "编辑模式")

  ![命令模式](https://jupyter-notebook.readthedocs.io/en/latest/_images/command_mode.png "命令模式")



## 编辑模式

&emsp;&emsp;编辑模式可以打代码或写markdown。Jupyer具有自动补全功能：代码打到一半后，按 *Tab* 就会自动补全或跳出候选框。



## 命令模式

&emsp;&emsp;按 *Esc* 退出编辑模式，进入命令模式。下面着重说一下常用快捷键：

| 快捷键 | 功能 |
| ------ | ---- |
| *S* | 保存 |
| ↑ / *K*  和  ↓ / *J* | 上下选择要操作的块 |
| *Enter* |  进入编辑模式 |
| *Esc* | 退出编辑模式 |
| *Ctrl + Enter* | 运行代码块/显示编辑效果 |
| *Shift + Enter* | 运行代码块，并且光标跳到下一代码块。如果已经是最后一个代码块，则会新建一个代码块 |
| *Alt + Enter* | 运行代码块，并在下方新建一个代码块 |
| *A* | 在上方新建代码块 |
| *B* | 在下方新建代码块 |
| 双击 *D* | 删除 |
| *Z* | 撤销已删除的块 |
| *M* | 将代码块转化为 markdown块 |
| *Y* | 将 markdown块转化为代码块 |

&emsp;&emsp;更多快捷键可以去 Help 中查询。还有一些命令是没有快捷键的，需要按 *P* 建进入命令输入框。比如 如果想显示行号，可以输入 line，就能看到 show all line numbers 的命令，选中，回车。



## 程序与内核

&emsp;&emsp;运行程序时，可能会有个问题，就是：不同代码块之间的程序是否相互影响？

&emsp;&emsp;实际上，Jupyter 比较类似与 Python 的交互模式。在同一个内核中，先运行的代码块会影响后运行的代码块。代码块的先后可以通过界面左边的 In[ ] 中的数字判断。若要清除前面已运行的代码块的影响，可以重启内核，点击界面上的 :stop_button: 或用快捷键 *0* （零）



# 进阶

## 执行Shell命令

&emsp;&emsp;在代码块中，在Shell命令前加`!`即可。甚至可以在 Shell 与 Python 之间传递值，比如

```python
path = !pwd
print(path)
```



## 换主题

&emsp;&emsp;在 Linux 上 pip 安装 `sudo pip install jupyterthemes`，然后加载可用主题 `jt -l` ，最后选择主题 `jt -t <name of theme>`，重启 Jupyter 即可。想恢复原主题，可用 `jt -r`



## 扩展

```shell
sudo pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install
```

&emsp;&emsp;安装完后，就能体验 候选语句、目录、折叠、自动整理等功能了，就在菜单 Edit 那里。



# 高级

&emsp;&emsp;现在有了一个新的 JupyterLab，可以去玩一下。



其他：

[Jupyter notebook 查看Markdown .md文件](https://blog.csdn.net/tuzixini/article/details/83117542)

