---
layout: post
#标题配置
title:  树莓派用作HID设备
#时间配置
date:   2019-03-17 15:33:00 +0800
#大类配置
categories: raspberry
#小类配置
tag: 教程
---



* content
{:toc}
# 什么是HID

​	HID（Human Interface Device）是指人机交互设备，如鼠标、键盘、游戏手柄。说是人机交互，但你可以没有人机接口，只要符合[HID设备协议](https://www.usb.org/sites/default/files/documents/hut1_12v2.pdf)就行。

# HID设备协议

​	具体可以去[USB-IF组织](https://www.usb.org/)上查阅[HID Usage Tables 1,12](https://www.usb.org/document-library/hid-usage-tables-112)文档，但USB相关的文档长到够读一个月了，就从网上整理了一些资料。





# 参考文档

以下是这篇文章的参考网站，为便于查看用网页内容命名，点进去可查看具体网页。

* [HID Usage Tables 1,12](https://www.usb.org/document-library/hid-usage-tables-112) 
* [使用树莓派 Zero 实现带回显的新型 Bad USB](http://shumeipai.nxez.com/2018/06/26/using-raspberry-pi-zero-to-implement-new-bad-usb-with-echo.html)
* [Arduino模拟电脑键盘](https://blog.csdn.net/yinkaishikd/article/details/49680629)
* [Easy USB 51 Programer Plus](http://usb.baiheee.com/usb_projects/easy_usb_51_programer_plus/easy_usb_brief.html)
* [树莓派可以做 USB HID吗？](http://www.icxbk.com/ask/detail/16706.html)
* [P4wnP1](https://github.com/mame82/P4wnP1)
* [What exactly are the differences between a USB host and device](https://electronics.stackexchange.com/questions/49140/what-exactly-are-the-differences-between-a-usb-host-and-device)