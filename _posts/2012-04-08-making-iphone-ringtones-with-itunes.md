---
title: 用 iTunes 制作 iPhone 铃声
author: James Shih
layout: post
permalink: /2012/04/08/making-iphone-ringtones-with-itunes/
categories:
  - Apple
  - iOS
tags:
  - DIY
  - iPhone
  - iTunes
  - Ringtones
  - 铃声
---
因为我很喜欢诺基亚那个铃声，最近把 iPhone 的铃声设置成了“当当当当-当当当当-当当当当当”，效果不错还有点雷人。我是把我以前手机里的铃声提取出来之后，用 iTunes 制作成 iPhone 铃声的。

iPhone 在不越狱的情况下，想 DIY 铃声就只能通过 iTunes 来进行，而且长度不能超过 40 秒否则 iPhone 不能使用（<del datetime="2012-06-19T07:47:28+00:00">这是很广泛也比较古老的说法，但这一点我并没有实际验证过，也许现在的 iOS 版本就没有此限制</del> **Update(06/19/2012)：**很不幸，经验证，iOS 5.1.1 仍有此限制，不过 [iOS 6 上似乎可以挑手机上的歌曲作为铃声？][1]）。具体的方法，网上很多地方都有，可以说是相当普及的知识了。但我阅读到的很多文章比较乱，不完整，或者没有图片，于是我整理了一个自己的版本。

<!--more-->

1. 首先，将铃声素材（MP3、WAV、AAC 等格式均可）导入 iTunes。如果你的铃声素材是 iTunes 无法识别的格式，推荐使用 foobar 2000 配合 converter 插件来转换成 AAC 格式。把文件拖放到 iTunes 左上角的**资料库**即可。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_1.png" alt="20111012_iphone_ringtone_diy_1">
</figure>

2. 然后，截取素材中的一段作为铃声。在**资料库**中点击**音乐**，在右侧列表中找到刚刚导入的素材，右键单击并选择**显示简介**。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_2.png" alt="20111012_iphone_ringtone_diy_2">
</figure>

3. 选择**选项**选项卡，通过**音量调整**可以放大或缩小铃声的音量，通过**起始时间**和**停止时间**可以设置素材中的一段作为铃声。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_3.png" alt="20111012_iphone_ringtone_diy_3">
</figure>

4. 接着，我们要将铃声转换为 AAC 或 Apple Lossless 格式。一般来说，作为铃声的话，最高质量的 AAC 格式足矣。如果对音质没有特殊的要求，可以跳过此步。按 **Ctrl+,** 打开**偏好设置**，在**常规**选项卡中单击**导入设置&#8230;**按钮，选择 **AAC 编码器**或 **Apple Lossless 编码器**。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_4.png" alt="20111012_iphone_ringtone_diy_4">
</figure>

5. 右键单击素材，选择**创建 AAC 版本**或**创建 Apple Lossless 版本**。iTunes 会制作出铃声文件，并自动放入 iTunes 资料库。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_5.png" alt="20111012_iphone_ringtone_diy_5">
</figure>

6. 右键单击刚制作出的铃声文件，选择**在 Windows 资源管理器中显示**。然后将铃声文件的扩展名改为 **m4r**。再将改好的文件拖放回 iTunes 资料库。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_6.png" alt="20111012_iphone_ringtone_diy_6">
</figure>

7. 这时，在**资料库**中点击**铃声**，就能看到制作好的铃声了。将你的 iPhone 连接到电脑，将铃声拖放到 iPhone 上就行了。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_7.png" alt="20111012_iphone_ringtone_diy_7">
</figure>

8. 最后，在 iPhone 的**设置**->**声音**->**电话铃声**中设置铃声。

<figure>
  <img src="/media/legacy/2012/04/20111012_iphone_ringtone_diy_0.png" srcset="/media/legacy/2012/04/20111012_iphone_ringtone_diy_0.png 2x" alt="20111012_iphone_ringtone_diy_0">
</figure>

 [1]: http://www.gizmodo.co.uk/2012/06/the-best-ios-6-features-nobody-is-talking-about/
