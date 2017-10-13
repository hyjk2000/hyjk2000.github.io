---
layout: post
title: 让旧 iPhone 变成 AirPlay 接收器
quote: 想要 AirPlay 无线音箱？只要手上有一只旧 iPhone 加上任何带 3.5mm AUX 输入的音箱
date: "2017-10-13 16:56:12 +0800"
slug: using-iphone-as-airplay-receiver
author: James Shih
categories:
  - Gadgets
tags:
  - AirPlay
  - 旧物利用
  - iPhone
bg_url: /media/2017/2017-10-13-using-iphone-as-airplay-receiver/AirFloat-0.png
---
想要 AirPlay 无线音箱？只要手上有一只旧 iPhone 加上任何带 3.5mm AUX 输入的音箱，你就可以自制一个，而且方法非常简单。

<figure>
  <img alt="AirFloat in action" src="/media/2017/2017-10-13-using-iphone-as-airplay-receiver/AirFloat-1.png" srcset="2x /media/2017/2017-10-13-using-iphone-as-airplay-receiver/AirFloat-1.png">
  <figcaption>AirFloat</figcaption>
</figure>

[AirFloat](https://github.com/trenskow/AirFloat) 是一个 iOS 上的 AirPlay 接收器实现。虽然因为作者是从头实现了 AirPlay audio 标准，难免会有不稳定的情况出现，但就我的测试结果来看，AirFloat 是完全可用的。出于显而易见的原因，它只能在越狱的 iPhone 上安装。不过对于一只[降级到 iOS 6 的 iPhone 4s](https://www.cydiageeks.com/odysseusota-downgrade-iphone-ipad-ios-6-1-3-without-shsh.html) 来说，越狱是[很容易的事情](https://canijailbreak.com/)。

<figure>
  <img alt="AirFloat streaming music" src="/media/2017/2017-10-13-using-iphone-as-airplay-receiver/AirFloat-2.png" srcset="2x /media/2017/2017-10-13-using-iphone-as-airplay-receiver/AirFloat-2.png">
  <figcaption>AirFloat 接收来自 iPhone 的音乐</figcaption>
</figure>

在 Cydia 上搜索安装 AirFloat，直接运行，同一个 Wi-Fi 下的设备就可以看到 AirPlay 目标设备了。你可以从 iOS 设备或者 Mac/PC 上的 iTunes 连接到 AirFloat。如果你有好几个旧 iPhone 安装了 AirFloat，用 iTunes 甚至可以同时连接到所有设备，同步播放！
