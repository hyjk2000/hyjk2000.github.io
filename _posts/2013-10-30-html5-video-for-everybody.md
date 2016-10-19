---
title: 给每个人的 HTML5 视频
quote: 要想让你的所有用户都能看到你发布的视频，你必须遵循一个标准的发布方法。
author: James Shih
layout: post
permalink: /2013/10/30/html5-video-for-everybody/
authorsure_include_css:
  -
categories:
  - HTML/CSS
  - Web 开发
tags:
  - HTML5
  - WebVTT
  - 浏览器兼容
  - 编解码器
  - 视频
  - 视频容器
---
视频是 HTML5 中最受欢迎的特性之一。跟以前调用插件的做法相比，只要一个 `<video>` 就行的便利实在是今非昔比。除此之外，HTML5 视频对移动设备的友好也是 Flash 难望项背的。到了 2013 年，浏览器和各种移动设备对 HTML5 视频的支持已经相当成熟，尤其是移动设备上，HTML5 几乎是唯一实用的网页视频发布方式。

不过，HTML5 视频有个很大的问题：兼容性。固执地坚守老旧浏览器的用户甚至不算是这个问题的根源，而是 HTML5 规范中没有规定（也不可能规定）视频所用的容器和编解码技术，所以即使在不考虑缺乏 HTML5 支持的浏览器时，各种视频技术涉及的大量规格和授权类型，使不同的浏览器（甚至同一浏览器在不同平台上的版本）对 HTML5 视频的兼容性差别极大。要想让你的所有用户都能看到你发布的视频，你必须遵循一个标准的发布方法，而这就是本文的目的。

<!--more-->

### 容器和编解码器

要发布兼容度最好的视频，有必要了解常用的视频容器和编解码器，以及各平台和浏览器的支持程度。

当我们看一个“视频”的时候，其实我们是在看视频，同时在听音频。一个视频文件中，至少储存了两条数据：一条是视频轨道、一条是音频轨道。这些数据封装在一个叫 **容器** 的文件格式中。MP4、MKV、AVI 等都是我们熟悉的容器格式，但是它们都只是装东西的袋子，还不能决定一个视频能否被支持。

当我们播放视频时，分离器会解析容器，找到其中的视频和音频数据流，根据它们的类型把它们传给相应的 **编解码器**。编解码器会解码这些数据流并通过播放器和操作系统输出到屏幕和扬声器上。

由此可见，播放器必须能识别容器，并拥有视频和音频数据流相应的编解码器，才能播放视频。浏览器和操作系统对视频的支持，其实就是对容器以及视频和音频编解码器组合的支持。

### Web 支持的容器和编解码器

尽管视频容器和编解码器多如牛毛，但因为授权、效率或硬件解码等原因绝大部分在 Web 上都不受支持。如今在 Web 上常见的组合不外乎三种：

##### H.264 + AAC + MP4

[H.264][7] 也称为 MPEG-4 Part 10 或 MPEG-4 AVC，由 [动态图像专家组][8] 开发。这种格式可以应用在从低带宽、低性能的移动设备到高带宽、高性能的台式电脑的所有设备上，因此，它被划分为多个 [Profiles][9] 和 [Levels][10]，它们规定了编码的细节规格，以在编码效率和编解码所需计算性能之间取得平衡。例如手机等移动设备普遍可以支持 Baseline Profile（较新的设备如 iPhone、iPad 等也可支持 Main 甚至 High Profile），电脑则可以支持 Baseline、Main 和 High Profile。

H.264 的最大优点是支持硬件解码。在 iPhone 等移动设备以及较新的电脑上，有专门的硬件模块用于 H.264 的编解码工作，这可以减少不擅此道的 CPU 的负担，大大降低能耗并提高性能，这对 CPU 性能和电池容量有限的移动设备来说至关重要。不过 H.264 是一种专利技术，从法律上讲，要发布通过 H.264 编码的视频，需要有 [MPEG LA Group][11] 的授权。

[AAC][12] 是作为代替 [MP3][13] 的格式开发的，拥有远胜过 MP3 的编码效率和多声道支持。与 H.264 相似，AAC 也分为针对一般设备的 LC-AAC 和针对高性能设备的 HE-AAC。Web 上绝大部分都使用 LC-AAC。

AAC 有非常广泛的支持，iPhone 等移动设备，QuickTime、Windows Media Player 等闭源播放器，和 VLC 等开源播放器都支持 AAC。另外，AAC 同样也是一项专利技术，使用需要授权。

[MP4][14] 是从苹果的 [QuickTime][15] 的规格上发展而来的容器。MP4 视频有 mp4 和 m4v 两种常见的扩展名，后者是苹果为了与纯音频的 MP4 文件（m4a）区分而创造的。

##### WebM

[WebM][16] 是专门为 HTML5 视频设计的，由 Google 赞助开发，是开源、有免授权费专利的技术。目前，它由 [VP8][17] 视频编码、[Vorbis][18] 音频编码和一种基于 [Matroska][19] 的容器组成。

VP8 原来由 [On2][20] 所开发，是一种与 H.264 非常类似的编码格式（性能也很接近），Google 在 2010 年收购 On2 后便将 VP8 开放源代码。

Vorbis 是由 [Xiph.Org 基金会][21] 维护的开源音频编码格式，目的是与有专利限制的 MP3 和 AAC 竞争，是开源世界最成功的音频编码格式之一。它的编码效率可以与 AAC 相比，大大高于 MP3。

WebM 最近已新增了 [VP9][22] 视频编码和 [Opus][23] 音频编码的 WebM with VP9 版本，Chrome 30 开始已经可以支持。但目前除了基于 Webkit 的几种桌面浏览器（Chrome、Safari、Opera）的较新版本外，还几乎没有其他浏览器和平台支持 WebM with VP9。

Chrome、Firefox、Opera 原生支持 WebM，IE 和 Safari 则需要安装编解码器。WebM 视频的扩展名一般是 webm。

##### Theora + Vorbis + Ogg

[Ogg][24] 现由 Xiph.Org 基金会维护，也是开源、有免授权费专利的技术，但它的历史比 WebM 更久。它包含 [Theora][25] 视频编码、 [Vorbis][18] 音频编码和 [Ogg][24] 容器。

Theora 源自 VP3，与 WebM 的 VP8 一样都由 On2 所开发，不过 VP3 早在 2001 年就开放源代码了。

Chrome、Firefox、Opera 以及所有的主要 Linux 发行版原生支持 Ogg。Ogg 视频的扩展名一般是 ogv。

以下是未安装任何附加组件的情况下，主要浏览器和移动设备对以上三种组合的支持程度：

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th>IE</th>
      <th>Chrome</th>
      <th>Firefox</th>
      <th>Opera</th>
      <th>Safari</th>
      <th>iOS</th>
      <th>Android</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>H.264 + <abbr title="Advanced Audio Coding">AAC</abbr> + MP4</th>
      <td>9+</td>
      <td>4+</td>
      <td>21+ <sup>1</sup></td>
      <td>-</td>
      <td>3+</td>
      <td>3+</td>
      <td>2.1+ <sup>2</sup></td>
    </tr>
    <tr>
      <th>WebM</th>
      <td>-</td>
      <td>6+</td>
      <td>4+</td>
      <td>10.6+</td>
      <td>-</td>
      <td>-</td>
      <td>2.3+</td>
    </tr>
    <tr>
      <th>Theora + Vorbis + Ogg</th>
      <td>-</td>
      <td>4+</td>
      <td>3.5+</td>
      <td>10.5+</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
    </tr>
  </tbody>
</table>

<sup>1</sup>：在 Windows 7 以上支持，在 Mac 和 Linux 上不支持
<sup>2</sup>：播放时需要[特殊处理][26]

### 如何做到最大兼容

从上表可以看出，没有一种格式可以兼容所有的情况，必须使用至少两种组合。

*   要支持移动设备、IE 和 Mac，H.264 + AAC + MP4 是必须的
*   要支持开源平台，需要 WebM 或 Theora + Vorbis + Ogg

除了 H.264 + AAC + MP4 必须使用外，WebM 和 Theora + Vorbis + Ogg 的支持范围比较相近，只不过前者的技术更新，压缩效率更高，而后者的历史更久，在较老的平台上也受到支持。可酌情挑选其中一种。以下是三种都使用的例子：

```html
<video width="640" height="360" controls>
    <source src="code-rush.m4v" type='video/mp4; codecs="avc1.42E01E,mp4a.40.2"'>
    <source src="code-rush.webm" type='video/webm; codecs="vp8,vorbis"'>
    <source src="code-rush.ogv" type='video/ogg; codecs="theora,vorbis"'>
</video>
```

在这里要解释一下 `<source>` 里的 `type` 是视频文件的 **MIME Type**，它描述了视频的容器和编码类型。也可以不写 `codecs` 的内容，但写上可以让浏览器无需下载视频文件即可判断自己能否播放，从而选择正确的格式。

这里 MP4 的 `type` 比较复杂：

<dl>
  <dt><code>avc1.42E01E</code></dt>
  <dd>
    这是视频编码规格：<br>
    Baseline &#8211; avc1.42E0xx（xx 是 Level，下同）<br>
    Main &#8211; avc1.4D40xx<br>
    High &#8211; avc1.6400xx<br>
    Level 是以 16 进制表示的：如 Level 3 是 1E（30 的 16 进制），Level 4.1 是 29（41 的 16 进制）
  </dd>
  <dt><code>mp4a.40.2</code></dt>
  <dd>这是音频编码规格，代表 LC-AAC</dd>
</dl>

可以用 JavaScript 判断浏览器是否支持一种 MIME Type：

```javascript
var canPlayMP4 = document.createElement("video").canPlayType('video/mp4; codecs="avc1.42E01E,mp4a.40.2"');
// "probably"：浏览器认为它可以支持指定的容器和编码格式，应该可以播放
// "maybe"：浏览器认为它可以支持指定的容器，但不确定能否支持编码格式，也许可以播放
// ""：浏览器无法支持指定的容器
```

要测试你的浏览器对各种 MIME Type 的支持情况，<a href="http://playgroundclassic.applinzi.com//html5-video-audio-types/" target="_blank">点击此处</a>。

### 为 HTML5 制作视频

我们往往需要将视频素材转换成适合 HTML5 的格式。[Handbrake][27] 和 [FFmpeg][28] 是常用的工具，它们都是跨平台的开源软件。Handbrake 有图形界面和命令行两种使用方式，而 FFmpeg 是纯命令行的。此外，还有一些有趣的工具可供选择：[Miro Video Converter][29] 是傻瓜型的，可以转换我们提到的三种视频格式；[MediaCoder][30] 是一个国产的软件，支持的格式很多，用起来也最麻烦；[Firefogg][31] 是一个 Firefox 扩展，可以制作 WebM 和 Ogg。

建议使用 FFmpeg，请在[官方下载页][32]找到自己操作系统的预编译包使用。Windows 64bit 和 OS X 用户也可以下载我编译的版本：

<a class="btn" href="http://pan.baidu.com/s/1paRxc#path=%252FFFmpeg%2520for%2520HTML5%2520Video" target="_blank">FFmpeg for HTML5 Video</a>

在 OS X 上可以用 [Homebrew][33] 来安装：

```bash
$ brew install ffmpeg --without-xvid --without-faac --without-lame --without-libvo-aacenc --with-libvpx --with-theora --with-fdk-aac --with-libvorbis --with-opus
```

制作 HTML5 视频时，需要考虑到用户的带宽。以 H.264 来说，常用画面规格的建议平均码率如下：

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th>320x240/24p</th>
      <th>640x360/24p</th>
      <th>854x480/24p</th>
      <th>1280x720/24p</th>
      <th>1920x1080/24p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>码率</th>
      <td>200kbps</td>
      <td>400kbps</td>
      <td>800kbps</td>
      <td>1500kbps</td>
      <td>3000kbps</td>
    </tr>
    <tr>
      <th>Profile <sup>1</sup></th>
      <td>Baseline</td>
      <td>Main</td>
      <td>Main</td>
      <td>High</td>
      <td>High</td>
    </tr>
    <tr>
      <th>Level <sup>1</sup></th>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>3.1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>

<sup>1</sup>：H.264 的 Profile 和 Level 与分辨率、帧率、码率的关系可以参见[这张表格][10]，也可以让转换软件自动选择。

如果对画质要求较高，或使用了更高的帧率，或使用 Theora 时，需要稍微提高码率。

另外 H.264 和 VP8 都支持 2-pass 编码，第一 pass 是扫描整个素材，记录下素材的一些特征，第二 pass 才是真正的编码。这种方法虽然比较花时间，但可以取得比 1-pass 更好的效果，时间充裕的话建议使用。

对于音频编码，AAC 和 Vorbis 的立体声码率均以 160kbps 为佳。

##### 用 FFmpeg 制作 MP4 视频 &#8211; [文档][34]

```bash
$ ffmpeg -pass 1 -fastfirstpass 1 -i INPUT -c:v libx264 -an -f rawvideo -y /dev/null
$ ffmpeg -pass 2 -i INPUT \
         -c:v libx264 -s 1280x720 -b:v 1500k -profile:v high -level 3.1 \
         -c:a libfdk_aac -ac 2 -b:a 160k -movflags faststart OUTPUT.m4v
```

重要的参数：

<dl>
  <dt><code>-pass</code></dt>
  <dd>如果不使用 2-pass 编码，则只需要执行第二个命令，并去掉 <code>-pass 2</code></dd>
  <dt><code>-fastfirstpass</code></dt>
  <dd>相当于 Handbrake 里的 Turbo First Pass，可以加快第一 pass 的速度</dd>
  <dt><code>-an -f rawvideo -y /dev/null</code></dt>
  <dd>这是在第一 pass 中不处理音频，并丢弃输出</dd>
  <dt><code>-c:a libfdk_aac</code></dt>
  <dd>libfdk_aac 也就是 Fraunhofer FDK AAC，是 FFmpeg 目前可以调用的最好的 AAC 编码器，但它并不是完全开源的，如果你的 FFmpeg 编译时没有包括它，可以换用 libfaac 或 FFmpeg 自带的 aac (由于是试验性的所以要加参数 -strict experimental），效果都不如 libfdk_aac</dd>
  <dt><code>-movflags faststart</code></dt>
  <dd>对网络视频而言十分重要，这可以让视频在下载完之前就开始播放</dd>
</dl>

##### 用 FFmpeg 制作 WebM 视频 &#8211; [文档][35]

```bash
$ ffmpeg -pass 1 -i INPUT -c:v libvpx -an -f rawvideo -y /dev/null
$ ffmpeg -pass 2 -i INPUT \
         -c:v libvpx -s 1280x720 -b:v 1500k \
         -c:a libvorbis -ac 2 -b:a 160k OUTPUT.webm
```

##### 用 FFmpeg 制作 Ogg 视频 &#8211; [文档][36]

```bash
$ ffmpeg -i INPUT \
         -c:v libtheora -s 1280x720 -b:v 1500k \
         -c:a libvorbis -ac 2 -b:a 160k OUTPUT.ogv
```

FFmpeg 的详细用法请详见[官方文档][37]。

### 添加字幕

HTML5 视频可以添加 [WebVTT][38] 字幕轨道，方法是在 `<video>` 内加上 `<track>`：

```html
<video width="640" height="360" controls>
    <source src="code-rush.m4v" type='video/mp4; codecs="avc1.42E01E,mp4a.40.2"'>
    <source src="code-rush.webm" type='video/webm; codecs="vp8,vorbis"'>
    <source src="code-rush.ogv" type='video/ogg; codecs="theora,vorbis"'>
    <track kind="subtitles" label="中文字幕" src="code-rush.chs.vtt" srclang="zh" default></track>
    <track kind="subtitles" label="英文字幕" src="code-rush.eng.vtt" srclang="en"></track>
</video>
```

以下是字幕文件的内容示例：

```
WEBVTT

1
00:00:02.150 --> 00:00:12.150
从1998年3月到1999年4月
一个独立纪录片摄制组跟随着网景公司的一个软件工程师团队
记录了他们公司和互联网历史上的分水岭

2
00:00:16.970 --> 00:00:20.240
我和很多到这里来寻找

3
00:00:20.240 --> 00:00:22.050
硅谷体验的人都聊过
```

事实上，它的格式和 SRT 字幕几乎一样，只不过秒和毫秒之间是点，而不是 SRT 的逗号，另外第一行要加上一个 WEBVTT 标识。

WebVTT 是非常新的技术，目前还处于草案阶段。以下是当前主要浏览器和移动设备的支持情况：

<table>
  <thead>
    <tr>
      <th>&nbsp;</th>
      <th>IE</th>
      <th>Chrome</th>
      <th>Firefox</th>
      <th>Opera</th>
      <th>Safari</th>
      <th>iOS</th>
      <th>Android</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>WebVTT</th>
      <td>10+</td>
      <td>18+</td>
      <td>24+ <sup>1</sup></td>
      <td>15+</td>
      <td>6+</td>
      <td>7+</td>
      <td>4.4+</td>
    </tr>
  </tbody>
</table>

<sup>1</sup>：默认禁用，在 `about:config` 中打开 `media.webvtt.enabled` 设置项来启用

要详细了解 WebVTT，可以看看[这篇文章][39]。

### 不支持 HTML5

当然是 Flash fallback 了：

```html
<video width="640" height="360" controls>
  <source src="code-rush.mp4" type='video/mp4; codecs="avc1.42E01E,mp4a.40.2"'>
  <source src="code-rush.webm" type='video/webm; codecs="vp8,vorbis"'>
  <source src="code-rush.ogv" type='video/ogg; codecs="theora,vorbis"'>
  <track kind="subtitles" label="中文字幕" src="code-rush.chs.vtt" srclang="zh" default></track>
  <track kind="subtitles" label="英文字幕" src="code-rush.eng.vtt" srclang="en"></track>
  <object type="application/x-shockwave-flash" data="http://releases.flowplayer.org/swf/flowplayer-3.2.1.swf" width="640" height="360">
    <param name="movie" value="http://releases.flowplayer.org/swf/flowplayer-3.2.1.swf" />
    <param name="allowFullScreen" value="true" />
    <param name="wmode" value="transparent" />
    <param name="flashVars" value="config={'playlist':['code-rush_poster.jpg',{'url':'code-rush.mp4','autoPlay':false}]}" />
    <img alt="Code Rush" src="code-rush_poster.jpg" width="640" height="360" title="No video playback capabilities, please download the video below" />
  </object>
</video>
```

在 `<video>` 标签里，按往常一样加上 Flash 播放器即可。注意：Flash 支持播放 H.264 视频，所以不需要再制作一个 FLV 出来。

 [1]: #section_containers-and-codecs
 [2]: #section_containers-and-codecs-for-web
 [3]: #section_how-to-maximize-compatibility
 [4]: #section_prepare-your-video-for-html5
 [5]: #section_add-subtitles
 [6]: #section_html5-not-supported
 [7]: https://en.wikipedia.org/wiki/H.264
 [8]: https://en.wikipedia.org/wiki/Moving_Picture_Experts_Group
 [9]: https://en.wikipedia.org/wiki/H.264#Profiles
 [10]: https://en.wikipedia.org/wiki/H.264#Levels
 [11]: http://www.mpegla.com/
 [12]: https://en.wikipedia.org/wiki/Advanced_Audio_Coding
 [13]: https://en.wikipedia.org/wiki/MP3
 [14]: https://en.wikipedia.org/wiki/MPEG-4_Part_14
 [15]: https://en.wikipedia.org/wiki/QuickTime
 [16]: https://en.wikipedia.org/wiki/WebM
 [17]: https://en.wikipedia.org/wiki/VP8
 [18]: https://en.wikipedia.org/wiki/Vorbis
 [19]: https://en.wikipedia.org/wiki/Matroska
 [20]: https://en.wikipedia.org/wiki/On2
 [21]: https://en.wikipedia.org/wiki/Xiph.Org_Foundation
 [22]: https://en.wikipedia.org/wiki/VP9
 [23]: https://en.wikipedia.org/wiki/Opus_(audio_format)
 [24]: https://en.wikipedia.org/wiki/Ogg
 [25]: https://en.wikipedia.org/wiki/Theora
 [26]: http://www.broken-links.com/2010/07/08/making-html5-video-work-on-android-phones/
 [27]: http://handbrake.fr/
 [28]: http://www.ffmpeg.org/
 [29]: http://www.mirovideoconverter.com/
 [30]: http://www.mediacoderhq.com/
 [31]: http://firefogg.org/
 [32]: http://www.ffmpeg.org/download.html
 [33]: http://brew.sh
 [34]: http://trac.ffmpeg.org/wiki/x264EncodingGuide
 [35]: http://trac.ffmpeg.org/wiki/vpxEncodingGuide
 [36]: http://trac.ffmpeg.org/wiki/TheoraVorbisEncodingGuide
 [37]: http://www.ffmpeg.org/ffmpeg-all.html
 [38]: http://dev.w3.org/html5/webvtt/
 [39]: https://dev.opera.com/articles/an-introduction-to-webvtt-and-track/

 *[AVC]: Advanced Video Coding
 *[AAC]: Advanced Audio Coding
 *[MP3]: MPEG-1 or MPEG-2 Audio Layer III
 *[LC-AAC]: Low Complexity AAC
 *[HE-AAC]: High Efficiency AAC
