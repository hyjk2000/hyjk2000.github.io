---
title: iOS 8 的屏幕录像功能
quote: iOS 8 带来了一个鲜为人知的新功能，给开发者带来了便利
author: James Shih
layout: post
permalink: /2014/10/21/ios-8-screen-recording/
categories:
  - Apple
  - iOS
tags:
  - Mac
  - iPhone
  - iPad
---
iOS 设备的屏幕录像一直是不太方便的，开发者录制视频时，往往还处在用摄像机直接翻拍屏幕的阶段。而 iOS 8 带来了一个鲜为人知的新功能，给开发者带来了便利。只需要把设备连接到 Mac，利用 QuickTime Player 就可以方便地录制了。

利用 QuickTime Player 录制 Mac 的屏幕录像，很多人应该都很熟悉了。给 iOS 8 设备录像同样很简单。打开 QuickTime Player，在 **文件** 菜单中选择 **新建影片录制**，或按 <kbd title="Option + Command + N">⌥⌘N</kbd>：

<figure>
  <img src="/media/2014/2014-10-21-ios-8-screen-recording/1.png" alt="新建影片录制">
</figure>

接着，点击录制按钮右边的箭头，选择你的 iOS 设备作为相机和麦克风，QuickTime Player 就会显示出设备上的画面，点击录制即可开始录制：

<figure>
  <img src="/media/2014/2014-10-21-ios-8-screen-recording/2.png" alt="选择 iOS 设备">
</figure>

此时，iOS 设备上的音频也会被录制。如果你想在演示中加上旁白，也可以选择 Mac 的内建麦克风作为音频源。

录制的视频格式仍然是 QuickTime 的 mov 格式，压缩率并不是很高。可以参考[这篇文章](/2013/10/30/html5-video-for-everybody/)来转换成其他格式，方便传播。

这是我用 iOS 8 的屏幕录像功能录制的 Zen Garden 演示视频，可见录制过程对 iPhone 的性能影响微乎其微：

<figure>
  <iframe height="498" width="510" src="http://player.youku.com/embed/XNzg4NjU4NDcy" frameborder="0" allowfullscreen="true"></iframe>
</figure>

细心的你可能会注意到，iOS 设备顶部的状态栏有了一些变化：

<figure>
  <img src="/media/2014/2014-10-21-ios-8-screen-recording/3.png" srcset="/media/2014/2014-10-21-ios-8-screen-recording/3.png 2x" alt="状态栏的变化">
</figure>

没错，蜂窝移动网络和 Wi-Fi 信号强度显示为满格、运营商名称被隐藏、时间显示为上午 9:41、电池电量也显示为 100%，完全和 Apple 官方的宣传图和视频一样。这样对细节的关注，是不是很 Apple 呢？
