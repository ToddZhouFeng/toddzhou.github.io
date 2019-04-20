---
layout: post
title:  初次尝试用Github搭建博客
date:   2019-03-17 14:00:00 +0800
categories: diary
tag: 教程
music-id: 465675773
---

* content
{:toc}
# 为什么要搭博客？

​	博客就是QQ空间、微博、朋友圈、公众号的祖宗，但这些晚辈大多太零碎，如果你希望自己的文章能够保持连贯性，那么还是得用回1995年发明得博客。别觉得博客老土。把它看作一个网站，专属于你的网站。你可以自由地编辑网站的样式，在网页上添加各种动画图片，把它弄成你的风格。这样一想，博客貌似挺cool的。

​	传统搭博客的方法一般是去新浪博客或其他网站申请一个账号，然后往里面添加文章。但它的版式就那几种，不能自己定制，特难看。而GIthub的博客是全定制的，如果你厉害，可以自己写网页；又或者，如果你嫌麻烦，可以用别人编好的主题。这些主题来自于全世界，数量也很多，总有一款是适合你的。对了，虽然功能很强大，但Github的博客依然是免费的。

# 如何用Github搭博客

​	首先申请Github账号，然后创建一个“账号名+github.io”的repo，然后设置里就会出现相关设置，按它的步骤写就行了。具体步骤请看[傻瓜都可以利用github pages建博客](http://cyzus.github.io/2015/06/21/github-build-blog/)，我就是照着上面来做的。前面这些过程花费不到半小时。

​	然后特别操蛋的来了，下载了jekyll主题后，你需要一个一个修改页面上的文字，有时候根本找不到要修改的文件，这个花了我一个中午的时间去找......这里有一个特别的技巧，就是用浏览器打开博客，然后在空白处点击右键，选则“检查”，这样就可以找到相关文件了。

​	修改好后，把它提交到Github仓库，然后去setting检查一下，如果你的内容有错，它会在setting那里指示出来，按提示修改就行了。如果没问题，过1~2分钟后就可以看到自己的博客了。

# 记录我使用的博客主题

​	我用的是[LessOrMore](https://github.com/luoyan35714/LessOrMore.git)主题，是由国人编写的，对中文支持很好，版式很简洁。大部分修改要修改的文件在根目录和\_includes文件夹里。我把原作者的一些链接改成了我社交媒体的链接（其实就只有Github和B站）。根目录的\_config.yml修改格式如下：

```bash
name: 博客名称
email: 邮箱地址
author: 作者名
url: 个人网站
### baseurl修改为项目名，如果项目是'***.github.io'，则设置为空''
baseurl: "/LessOrMore"
resume_site: 个人简历网站
github: github地址
github_username: github用户名称
FB:
  comments :
    provider : duoshuo
    duoshuo:
        short_name : 多说账户
    disqus :
        short_name : Disqus账户
```



​	每次新写文章，都要在开头加上：

```bash
---
layout: post
#标题配置
title:  标题
#时间配置
date:   2019-08-27 01:08:00 +0800
#大类配置
categories: document
#小类配置
tag: 教程
#网易云音乐，只能播放无版权保护的
music-id: 465675773
---

* content
{:toc}


我是正文。我是正文。我是正文。我是正文。我是正文。我是正文。
```

如果想要在文章里插入图片，可以这样写（路径为github内的路径）：

```markdown
![诫子书]({{ '/styles/images/jiezishu.jpg' | prepend: site.baseurl  }})
```

效果是这样的（这个只能在网页上看，本地的Markdown编辑器不支持）

![诫子书]({{ '/styles/images/jiezishu.jpg' | prepend: site.baseurl  }} "诫子书")

更多信息需要看[LessOrMore的Github](https://github.com/luoyan35714/LessOrMore.git)。



顺便说一句，原作者是不支持音乐的。但我可以（下面的是《西安人的歌》）：

<p>http://yizhibi.6chemical.com/lucyBlog/xianrendege.mp3</p>

参考了[一支笔](<https://yizibi.github.io/2018/10/15/jekyll%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%B7%BB%E5%8A%A0%E9%9F%B3%E4%B9%90%E6%92%AD%E6%94%BE%E6%8F%92%E4%BB%B6/>)的博客，只需要在对应位置输入：

~~~html
<p>音乐外链url</p>
~~~

缺点是必须是音乐文件（mp3√，其他未知），可能需要云端储存。



下面时B站的框架，不知道播不播得了视频...



<div class="embed-responsive embed-responsive-4by3">
<iframe src="//player.bilibili.com/player.html?aid=45368874&cid=79420762&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
</div>

