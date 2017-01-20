---
title: 关于 Power Nap
author: James Shih
layout: post
permalink: /2012/08/10/all-about-power-nap/
categories:
  - Apple
  - Mac
  - OS X
tags:
  - Mountain Lion
  - OS X
  - Power Nap
bg_url: /media/legacy/2012/08/20120810_all-about-power-nap.jpg
bg_author: pauldavidy / flickr
bg_source: https://www.flickr.com/photos/pauldavidy/5643448293/
---
Apple 在 OS X Mountain Lion 中新增了一个很新鲜的功能：Power Nap，也就是“小睡”功能。这个功能可以让 Mac 在睡眠状态下做一些更新、同步的活儿，以便让我们在使用电脑时不需要再等着它更新。很多人对于它能做什么、费不费电不太了解，不敢启用它。于是我对此进行了一些研究。

<!--more-->

**哪些机型支持 Power Nap**

目前能够支持 Power Nap 的机型只有 MacBook Air (Mid 2011 & Mid 2012) 以及 MacBook Pro (Retina, Mid 2012) 而已。也许你已经注意到了，只有配备 SSD 硬盘的机型才可以支持 Power Nap。除了升级到 OS X Mountain Lion 以外，这些机型还需要安装 SMC 更新才能开启 Power Nap：

<table>
  <tr>
    <th>
      机型
    </th>
    
    <th>
      SMC 更新
    </th>
  </tr>
  
  <tr>
    <td>
      MacBook Air (Mid 2012)
    </td>
    
    <td>
      <a class="btn" href="http://support.apple.com/kb/DL1558" target="_blank">下载</a>
    </td>
  </tr>
  
  <tr>
    <td>
      MacBook Air (Mid 2011)
    </td>
    
    <td>
      <a class="btn" href="http://support.apple.com/kb/DL1560" target="_blank">下载</a>
    </td>
  </tr>
  
  <tr>
    <td>
      MacBook Pro (Retina, Mid 2012)
    </td>
    
    <td>
      <a class="btn" href="http://support.apple.com/kb/DL1559" target="_blank">下载</a>
    </td>
  </tr>
</table>

**Power Nap 能做什么**

简单来说，Power Nap 可以做以下一些事情：

1.  收取 Email
2.  iCloud 同步通讯录、日历、提醒事项、备忘录、文档、照片流
3.  Mac App Store 的软件更新（仅当连接到电源适配器时）
4.  Time Machine 备份（仅当连接到电源适配器时）
5.  Find My Mac
6.  Spotlight Search 索引创建（仅当连接到电源适配器时）

除了软件更新为每天检查一次，Mac App Store 下载项目为每周检查一次外，其他项目均为每小时进行一次。

Power Nap 在进行上述的活动时，系统仍然会保持静默状态，风扇不会运转、屏幕和指示灯都不会点亮。有时你会感觉到机身有稍微发热，但系统会在温度达到一定值时（例如你把笔记本电脑放在包里），就会暂停活动。另外，Power Nap 还会在电池电量低于 30% 时暂停。

**怎样设置 Power Nap**

在“系统偏好设置”->“节能器”中，可以调整使用电池和电源适配器时是否启用 Power Nap。默认设置是使用电池时禁用，使用电源适配器时启用。

<figure>
  <img src="/media/legacy/2012/08/20120809_mountain-lion-power-nap-settings.png" alt="Mountain Lion Power Nap Settings">
  <figcaption>Mountain Lion 节能器设置</figcaption>
</figure>

**Power Nap 费不费电**

可以明确地告诉你：Power Nap 费不了多少电。

可能有人会担心 Power Nap 会消耗很多电池电量，从而不想在使用电池时启用它。但其实这样的担心其实并没有太大必要，因为 Power Nap 在使用电池时，其活动并不像连接电源适配器时是连续进行的，而是每隔 1 小时运行几分钟；而且，在使用电池时，Time Machine 备份等几项能耗较多的操作不会运行。经过一段时间的试用，我发现 Power Nap 并不会大大增加电池电量消耗。不过，Power Nap 运行时，连接到电脑的设备（例如鼠标、U盘等）仍处于通电运行状态，所以这些设备会在 Power Nap 运行时消耗能量。

**综上，如果你拥有一台支持 Power Nap 的 Mac，放心大胆地启用它吧！**
