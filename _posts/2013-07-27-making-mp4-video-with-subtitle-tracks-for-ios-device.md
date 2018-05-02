---
title: 为 iOS 设备制作带字幕轨的 MP4 视频
quote: 事实上，iOS 设备是支持 MP4 内嵌字幕的。只要花上一点点时间，加上几款开源软件，你也可以制作出完美的内嵌字幕 MP4 视频。
author: James Shih
layout: post
permalink: /2013/07/27/making-mp4-video-with-subtitle-tracks-for-ios-device/
authorsure_include_css:
  - 
categories:
  - Apple
  - iOS
tags:
  - iOS
  - MKV
  - MKVtoolnix
  - MP4
  - MP4Box
  - subtitle
  - 字幕
image: /media/legacy/2013/07/20130727_making-mp4-video-with-subtitle-tracks-for-ios-device_1.png
---
iPad 和 iPhone 最棒的用途之一就是看电影了。本来我们可以在 iTunes Store 上方便地购买和租赁正版电影和电视节目，但杯具的是 iTunes Store 并不对中国开放。于是，除了 DVD 和蓝光光盘外，网络下载就成了获取内容的唯一途径。

由于网上下载到的视频多为 MKV 和 MP4，iOS 并不支持 MKV（除非你装了神器 [VLC for iOS][1]），MP4 又大多是直接加到视频里无法隐藏的硬字幕，或者根本没有字幕，iOS 又不支持外挂字幕。所以，要把从网上下载的视频放进 iOS 设备真是件麻烦的事。

事实上，iOS 设备是支持 MP4 内嵌字幕的，iTunes Store 上的电影也是用同样的方法添加字幕的。只要花上一点点时间，加上几款开源软件，你也可以制作出完美的内嵌字幕 MP4 视频。

<!--more-->

你需要以下原材料：

1.  **MP4 或 MKV**：这两种视频容器格式是当今网上最流行的，忘掉你爸的 RMVB 和 AVI 吧。
2.  **视频轨道 － AVC/H.264 Baseline**：此编码格式压缩率高，而且可以直接在 iOS 设备上播放。如果是其他格式，则需要先进行转换。
3.  **音频轨道 － AAC LC**：也可以是 MP3 格式，但 MP3 格式在同码率下效果不如 AAC，而且无法存储 2 个以上的声道。
4.  **字幕轨道 － SRT**：MP4 内嵌字幕只支持纯文本，所以你还需要删除 SRT 字幕中的样式代码。另外，iOS 设备只支持 UTF-8 字符集的字幕文件，所以在使用前需要先转换。

在是用原材料以前，建议先在 [VLC][2] 等播放器中播放，测试字幕与视频是否相符，并且通过播放器的媒体属性查看工具检查各轨道是否符合要求，视频的帧率是多少。

你需要以下工具：

1.  **MP4Box**：MP4 容器打包工具，是 GPAC 的一部分，开源。<a class="btn" href="http://gpac.wp.mines-telecom.fr/downloads/" target="_blank">下载</a>
2.  **MKVToolNix**：MKV 容器的全方位工具，开源。<a class="btn" href="http://bunkus.org/videotools/mkvtoolnix/downloads.html" target="_blank">下载</a>
3.  视频转换工具（可选）：[Handbrake][3]、[MediaCoder][4] 等等。
4.  字幕编辑工具（可选）：[Aegisub][5]、[SRTEdit][6] 等等。

准备好以后就可以动手了。

### 安装 MP4Box 和 MKVToolNix

这一步无需多言。Mac 用户下载后，从 dmg 里把软件包拖到任意目录里就行了；Windows 用户下载后，解压放进任意文件夹就行了。

为了方便从命令行访问，Mac 用户可以在 `~/.profile` 文件里加上一行：

```bash
export PATH=$PATH:/[存放路径]/Osmo4.app/Contents/MacOS/:/[存放路径]/Mkvtoolnix.app/Contents/MacOS/`
```

Windows 用户可以在用户环境变量中将 PATH 设置为：

```bash
%PATH%;[存放路径]/GPAC;[存放路径]/Mkvtoolnix
```

### 如果原视频为 MP4

如果原视频为 MP4，非常简单，你只需要直接添加字幕轨：

```bash
$ MP4Box -add subtitle.chs.srt:lang=zho:group=2:hdlr="sbtl:tx3g" movie.mp4
Timed Text (SRT) import - text track 640 x 470, font Serif (size 18)
Saving movie.mp4: 0.500 secs Interleaving
```

参数中的 `lang=zho` 表示字幕语言为中文；`group=2` 表示将字幕轨划分到单独的群组，与视频和音频轨道隔开；`hdlr="sbtl:tx3g"` 表示指定字幕类型为 [tx3g][7]，也就是 iOS 设备支持的类型。

如需添加多条字幕轨，则需要将除第一条外的其他字幕轨禁用，方法是在参数最后加上一个 `disable`：

```bash
$ MP4Box -add subtitle.chs.srt:lang=zho:group=2:hdlr="sbtl:tx3g" -add subtitle.eng.srt:lang=eng:group=2:hdlr="sbtl:tx3g":disable movie.mp4
Timed Text (SRT) import - text track 640 x 470, font Serif (size 18)
Timed Text (SRT) import - text track 640 x 470, font Serif (size 18)
Saving movie.mp4: 0.500 secs Interleaving
```

完成后，使用 `MP4Box -info` 命令可以查看 MP4 文件所包含的轨道信息：

```bash
$ MP4Box -info movie.mp4
* Movie Info *
	Timescale 1000 - Duration 01:28:32.866
	Fragmented File no - 4 track(s)
	File suitable for progressive download (moov before mdat)
	File Brand isom - version 512
	Created: GMT Thu Jan  1 00:00:00 1970

File has root IOD (9 bytes)
Scene PL 0xff - Graphics PL 0xff - OD PL 0xff
Visual PL: AVC/H264 Profile (0x15)
Audio PL: AAC Profile @ Level 2 (0x29)
No streams included in root OD

iTunes Info:
	Encoder Software: Lavf52.39.2

Track # 1 Info - TrackID 1 - TimeScale 48 - Duration 01:27:36.666
Media Info: Language "Undetermined" - Type "vide:avc1" - 126160 samples
Visual Track layout: x=0 y=0 width=640 height=470
MPEG-4 Config: Visual Stream - ObjectTypeIndication 0x21
AVC/H264 Video - Visual Size 640 x 470
	AVC Info: 1 SPS - 1 PPS - Profile High @ Level 2.1
	NAL Unit length bits: 32
	Pixel Aspect Ratio 1:1 - Indicated track size 640 x 470
Self-synchronized

Track # 2 Info - TrackID 2 - TimeScale 44100 - Duration 01:27:37.253
Media Info: Language "Undetermined" - Type "soun:mp4a" - 226411 samples
MPEG-4 Config: Audio Stream - ObjectTypeIndication 0x40
MPEG-4 Audio MPEG-4 Audio AAC LC - 2 Channel(s) - SampleRate 44100
Synchronized on stream 1

Track # 3 Info - TrackID 3 - TimeScale 1000 - Duration 01:28:32.866
Media Info: Language "Chinese" - Type "sbtl:tx3g" - 794 samples
3GPP/MPEG-4 Timed Text - Size 640 x 470 - Translation X=0 Y=0 - Layer 0
Alternate Group ID 2

Track # 4 Info - TrackID 4 - TimeScale 1000 - Duration 01:28:32.837
Track is disabled
Media Info: Language "English" - Type "sbtl:tx3g" - 794 samples
3GPP/MPEG-4 Timed Text - Size 640 x 470 - Translation X=0 Y=0 - Layer 0
Alternate Group ID 2
```

### 如果原视频为 MKV

如果原视频为 MKV，需要先用 MKVToolNix 把其中的视频、音频、字幕轨道导出，再用 MP4Box 打包。

第一条命令是显示 MKV 文件中现有的轨道，在其中找到视频、音频、字幕轨道的 ID：

```bash
$ mkvinfo movie.mkv
+ EBML head
|+ EBML version: 1
|+ EBML read version: 1
|+ EBML maximum ID length: 4
|+ EBML maximum size length: 8
|+ Doc type: matroska
|+ Doc type version: 2
|+ Doc type read version: 2
+ Segment, size 351865625
|+ Seek head (subentries will be skipped)
|+ EbmlVoid (size: 4044)
|+ Segment information
| + Timecode scale: 1000000
| + Muxing application: libebml v1.3.0 + libmatroska v1.4.0
| + Writing application: mkvmerge v6.2.0 ('Promised Land') built on Apr 28 2013 18:50:30
| + Duration: 5312.866s (01:28:32.866)
| + Date: Sat Jul 27 02:46:42 2013 UTC
| + Segment UID: 0xf5 0x4b 0xe2 0xfa 0x0d 0xab 0x0e 0x9d 0xdc 0xac 0xb8 0x6c 0x83 0xe5 0xf6 0x82
|+ Segment tracks
| + A track
|  + Track number: 1 (track ID for mkvmerge &#038; mkvextract: 0)
|  + Track UID: 2837708993
|  + Track type: video
|  + Lacing flag: 0
|  + MinCache: 1
|  + Codec ID: V_MPEG4/ISO/AVC
|  + CodecPrivate, length 42 (h.264 profile: High @L2.1)
|  + Default duration: 41.667ms (24.000 frames/fields per second for a video track)
|  + Language: und
|  + Video track
|   + Pixel width: 640
|   + Pixel height: 470
|   + Display width: 640
|   + Display height: 470
| + A track
|  + Track number: 2 (track ID for mkvmerge &#038; mkvextract: 1)
|  + Track UID: 1697335311
|  + Track type: audio
|  + Codec ID: A_AAC
|  + CodecPrivate, length 2
|  + Default duration: 23.220ms (43.066 frames/fields per second for a video track)
|  + Language: und
|  + Audio track
|   + Sampling frequency: 44100
|   + Channels: 2
| + A track
|  + Track number: 3 (track ID for mkvmerge &#038; mkvextract: 2)
|  + Track UID: 519964485
|  + Track type: subtitles
|  + Lacing flag: 0
|  + Codec ID: S_TEXT/UTF8
|  + Language: chi
| + A track
|  + Track number: 4 (track ID for mkvmerge &#038; mkvextract: 3)
|  + Track UID: 1783810624
|  + Track type: subtitles
|  + Default flag: 0
|  + Lacing flag: 0
|  + Codec ID: S_TEXT/UTF8
|+ EbmlVoid (size: 1147)
|+ Cluster
```

第二条命令是将上述轨道导出到单独的文件：

```bash
$ mkvextract tracks movie.mkv 0:video.264 1:audio.m4a 2:subtitle.chs.srt 3:subtitle.eng.srt
Extracting track 0 with the CodecID 'V_MPEG4/ISO/AVC' to the file 'video.264'. Container format: AVC/h.264 elementary stream
Extracting track 1 with the CodecID 'A_AAC' to the file 'audio.m4a'. Container format: raw AAC file with ADTS headers
Extracting track 2 with the CodecID 'S_TEXT/UTF8' to the file 'subtitle.chs.srt'. Container format: SRT text subtitles
Extracting track 3 with the CodecID 'S_TEXT/UTF8' to the file 'subtitle.eng.srt'. Container format: SRT text subtitles
Progress: 100%
```

第三条命令是将这些轨道打包成 MP4，参数中的 `-fps 23.976` 不能漏掉，因为默认的帧率是 29.97，与电影常见的每秒 24 帧不符：

```bash
$ MP4Box -add video.264 -fps 23.976 -add audio.m4a -add subtitle.chs.srt:lang=zho:group=2:hdlr="sbtl:tx3g" -add subtitle.eng.srt:lang=eng:group=2:hdlr="sbtl:tx3g":disable movie.mp4
AVC-H264 import - frame size 640 x 470 at 23.976 FPS
AVC Import results: 126160 samples - Slices: 710 I 54274 P 71176 B - 0 SEI - 707 IDR
Stream uses forward prediction - stream CTS offset: 2 frames
AAC import  - sample rate 44100 - MPEG-4 audio - 2 channels
Timed Text (SRT) import - text track 640 x 470, font Serif (size 18)
Timed Text (SRT) import - text track 640 x 470, font Serif (size 18)
Saving to movie.mp4: 0.500 secs Interleaving
```

### FAQ

Q: 我对命令行有恐惧感，有没有图形界面的工具？
:   A: Mac 用户比较幸运，[Subler][8] 是一款开源的图形界面工具，专门用来为 MP4 文件添加字幕，除此之外它还有编辑 iTunes metadata 的功能。Windows 用户可以选择 [Yamb][9]。

Q: 我是 Linux 用户，可以完成本文的内容吗？
:   A: Linux 用户可以完全按照 Mac 的方法完成，除了没有 Mac 上的图形界面工具 Subler 可以用。不过没关系，命令行才是 Linuxer 的舞台。

Q: 怎样删除 MP4 中的字幕轨？
:   A: 首先用 `MP4Box -info` 命令查看 MP4 中包含的轨道信息，然后用 `MP4Box -rem [轨道ID1] -rem [轨道ID2]` 删除不需要的轨道。

Q: 怎样检查视频、音频轨道是否符合要求？
:   A: 对于 MKV，可以用 `mkvinfo` 命令查看到各轨道的详细信息。另外，[VLC][2] 等播放器中也可以查看到这些信息。

Q: 怎样检查字幕文件的编码？如果不是 UTF-8，怎样转换？
:   A: 对于 Mac 用户，这就是两条命令的事，不需要装任何软件。首先用 `file -I [文件名]` 检查文件的编码（例如 UTF-8 或 GBK），然后用 `iconv -f [原编码] -t utf-8 [原文件] > [新文件]` 即可完成转换。对于 Windows 用户，则需要借助 [Nodepad++][10] 等文本编辑软件。

 [1]: https://itunes.apple.com/cn/app/vlc-for-ios/id650377962?mt=8
 [2]: http://www.videolan.org/vlc/
 [3]: http://handbrake.fr
 [4]: http://mediacoder.com.cn
 [5]: http://www.aegisub.org
 [6]: http://www.mysilu.com/thread-745987-1-1.html
 [7]: http://en.wikipedia.org/wiki/MPEG-4_Part_17
 [8]: http://code.google.com/p/subler/
 [9]: http://yamb.unite-video.com
 [10]: http://notepad-plus-plus.org/
