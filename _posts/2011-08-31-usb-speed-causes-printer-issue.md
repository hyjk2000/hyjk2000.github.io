---
title: USB速度设置导致打印机无法打印
author: James Shih
layout: post
permalink: /2011/08/31/usb-speed-causes-printer-issue/
image: /media/legacy/2011/08/20110831_printer-usb-speed.jpg
categories:
tags:
  - hp
  - printer
  - usb
  - usb full speed
  - usb high speed
  - USB速度
  - 惠普
  - 打印机
  - 无法打印
---
USB 速度设置会导致打印机无法打印，这种问题你碰到过吗？

<!--more-->

最近公司添置了一台惠普彩色激光打印机，临时装在我的电脑上，于是别人托我打印一些文件。一开始我觉得安装打印机这种事情简直太简单，于是我答应说一杯茶功夫就成，结果这杯茶足足喝了3个小时……我把打印机连接到电脑后，Windows 7 就自动安装好了驱动。就在我心里暗赞 盖茨这次干得很漂亮的时候，麻烦出现了。无论我怎么重新启动电脑或者重新安装驱动程序，打印机就是一个字也不打。

于是就在反复折腾中，3个小时过去了。

其实在打印机等了很久之后打出一些乱码，我就已经开始怀疑打印机的 USB 连接线似乎有问题。但是，该死！我怎么就没有想到调整 USB 连接速度呢？于是第二天早晨，我打开打印机，在打印机的设置菜单里选择“Service -> USB Speed”，设置为“Full”，打印机自动重启后问题就解决了。

可能是打印机附带的 USB 线太长，或者质量不行。所以 USB High Speed 连接不稳定，必须在打印机上设置成 USB Full Speed 模式才行。不过惠普设计打印机时把 USB High Speed 作为默认设置，却配了一根这么不给力的 USB 线，还真是有点可笑。
