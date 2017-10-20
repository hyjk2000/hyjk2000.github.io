---
layout: post
title: 为 iOS 制作 HEVC 视频
quote: 想要 AirPlay 无线音箱？只要手上有一只旧 iPhone 加上任何带 3.5mm AUX 输入的音箱
date: "2017-10-20 21:54:16 +0800"
slug: encoding-ios-playable-hevc-video
author: James Shih
categories:
  - Apple
  - iOS
tags:
  - ffmpeg
  - mp4box
  - hevc
---
iOS 11 以及 macOS High Sierra 最广为人知的新特性之一就是对 [HEVC](https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding) (H.265) 的支持。这种更高效的视频编码格式据称能减少 40% 的文件体积，这对喜欢用手机拍摄视频，尤其是 4K 视频的用户来说是个好消息。但这个新功能对于以前拍摄的视频并没有帮助，因为新的操作系统不会自动帮你重新用 HEVC 编码那些视频。幸运的是，我们仍然可以用 [FFmpeg](https://www.ffmpeg.org/) 来做到。

## 安装 FFmpeg

在 Mac 上可以用 Homebrew 来安装：

```bash
$ brew install ffmpeg --with-x265
```

## 转码

```bash
$ ffmpeg -i input.mov -c:v libx265 -tag:v hvc1 -preset medium -crf 28 -c:a copy output.mp4
```

参数解释：

<dl>
  <dt><code>-c:v libx265</code></dt>
  <dd>选择 libx265 作为视频编码器</dd>
  <dt><code>-tag:v hvc1</code></dt>
  <dd>将视频轨道标记为 hvc1，而不是默认的 hev1，否则 macOS 和 iOS 不能播放</dd>
  <dt><code>-preset medium</code></dt>
  <dd>（可选）编码方法预设，默认是 medium，这个设置主要用于权衡编码速度和文件大小</dd>
  <dt><code>-crf 28</code></dt>
  <dd>（可选）恒定码率因数，默认是 28，这个设置决定需要的视频质量</dd>
  <dt><code>-c:a copy</code></dt>
  <dd>直接拷贝音频数据，不做修改</dd>
</dl>

关于可以自定的设置参数，请见 [Encode/H.265](https://trac.ffmpeg.org/wiki/Encode/H.265)。更多 FFmpeg 使用方法，可参考阅读[《给每个人的 HTML5 视频》](/2013/10/30/html5-video-for-everybody/)。

经过以上编码过程后，视频的体积会减少很多。我用以上参数压缩了一段用 GoPro 拍摄的，长度为 3 分钟的 1080/30p 视频，大小仅 20M 左右，且没有明显的画质损失。考虑到源文件 500M 左右的大小，十分惊人。如果提高码率，例如设置 `-crf 16`，可能肉眼就很难区分出和源文件的区别了。
