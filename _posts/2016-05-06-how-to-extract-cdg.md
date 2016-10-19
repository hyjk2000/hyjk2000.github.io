---
layout: post
title: 卡拉 OK CD+G 复活记
quote: 最近，我很幸运地找到了一张压箱底的 CD+G 卡拉 OK 光盘。怎样才能在今天的电脑上，把这老古董里的数据弄出来？
date: "2016-05-06 21:03:44 +0800"
slug: how-to-extract-cdg
author: James Shih
categories:
  - 娱乐
tags:
  - CD+G
  - 复古
---
1990 年代，曾经有一个昙花一现的流行，就是卡拉 OK [CD+G][cdg] 光盘。在著名的 CD 规范[彩虹书][rainbow-books]里，CD+G 在[红皮书][red-book]的修订本中。这种光盘除了有和普通 CD 相同的音轨以外，还能储存低分辨率图像信息，支持的播放器可以把这些图像回放到电视上，不支持的播放器也可以把它当作普通的 CD 播放。因为它能储存简单图像，并且出现很早（1985 年发行了第一张 CD+G），它主要被用于卡拉 OK 内容上。当它 90 年代出现在中国以后，没有能力负担昂贵的 [LD][laserdisc] 的一般家庭也可以在家 K 歌了。不过，因为不能储存真正的视频，它很快就被 [VCD][vcd] 以及之后的 [DVD][dvd] 取代了，所以听说过和用过的人并不多。

<figure>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/cdg-logo.svg" alt="CD+G Logo" style="max-width:200px">
  <figcaption>CD+G Logo (维基百科)</figcaption>
</figure>

最近，我很幸运地找到了一张压箱底的 CD+G 卡拉 OK 光盘，想到上一次看到这张光盘里的图像的时候，貌似我还没上小学，科技的发展已经不知经历了多少次沧海桑田。于是我就在网上搜索，怎样才能在今天的电脑上，把这老古董里的数据弄出来。

<figure>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-cdg-1.jpg" srcset="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-cdg-1.jpg 2x" alt="CD+G Disc">
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-cdg-2.jpg" srcset="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-cdg-2.jpg 2x" alt="CD+G Disc">
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-cdg-3.jpg" srcset="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-cdg-3.jpg 2x" alt="CD+G Disc">
  <figcaption>CD+G 卡拉 OK 精選金曲</figcaption>
</figure>

令人吃惊的是，虽然 CD+G 光盘多年前就已经在市场上难觅踪迹了，但它竟仍然以 [MP3+G][mp3g] 文件（本质上就是每首歌一个 MP3 加一个包含图像数据的 .cdg 文件）的形式在网上流行着，并且有专门的播放软件。看到这么多内容在网上流传，很自然地想到，一定有从 CD+G 光盘中抓取这些内容的工具。事实上，我还真找到了一个开源的工具，专门做这种事。

## 抓取

[cdgtools][cdgtools]，一个 Linux 上的用 Python 写成的 CD+G 工具包。它包含的 cdgrip 就是我要用的抓取工具。在使用这个工具以前，要先用 [cdrdao][cdrdao] 把光盘内容提取成 TOC/BIN 文件：

```bash
$ cdrdao read-cd --driver generic-mmc-raw --device /dev/cdroms/cdrom0 --read-subchan rw_raw mycd.toc
```

如果你用过 cdrdao，会注意到 `--read-subchan` 参数，这会让 cdrdao 读取[子通道][subcode]数据，也就是我们要的 CD+G 图像数据所在的地方。

然后再用 cdgrip 将内容转换成 MP3 和 .cdg 文件：

```bash
$ python cdgrip.py --with-cddb mycd.toc
```

加上 `--with-cddb` 参数后，cdgrip 会从 CDDB 上下载专辑和曲名信息。

## 播放

上网搜索能找到不少能播放 MP3+G 的卡拉 OK 软件。经过尝试，我选择了 [Karafun Player][karafun]，有趣的是这个播放器是 Windows 软件，而刚才的抓取工具却只支持 Linux。打开软件，在左边栏点击 Add a folder，选择刚才提取出的 MP3+G 文件所在的目录即可播放。

<figure>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karafun.png" alt="Karafun Player">
  <figcaption>Karafun Player</figcaption>
</figure>

然后，就没有然后了。我就看着这些很有潮流感的图片，听着这些很有潮流感的歌，坐上了去 90 年代的时光机。猜猜，这些都是什么歌？

<figure>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-1.png" alt="卡拉 OK 1"><br>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-2.png" alt="卡拉 OK 2"><br>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-3.png" alt="卡拉 OK 3"><br>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-4.png" alt="卡拉 OK 4"><br>
  <img src="/media/2016/2016-05-06-how-to-extract-cdg/karaoke-5.png" alt="卡拉 OK 5">
</figure>

答案：

<ul style="display:inline-block;-webkit-transform:rotate(180deg);transform:rotate(180deg)">
  <li>明明白白我的心</li>
  <li>大约在冬季</li>
  <li>明天你是否依然爱我</li>
  <li>你看你看月亮的脸</li>
  <li>来生缘</li>
</ul>

[cdg]: https://en.wikipedia.org/wiki/CD%2BG
[rainbow-books]: https://en.wikipedia.org/wiki/Rainbow_Books
[red-book]: https://en.wikipedia.org/wiki/Red_Book_(CD_standard)
[laserdisc]: https://en.wikipedia.org/wiki/LaserDisc
[vcd]: https://en.wikipedia.org/wiki/Video_CD
[dvd]: https://en.wikipedia.org/wiki/DVD
[mp3g]: https://en.wikipedia.org/wiki/MP3%2BG
[cdgtools]: http://www.kibosh.org/cdgtools/index.php
[cdrdao]: http://cdrdao.sourceforge.net/
[subcode]: https://en.wikipedia.org/wiki/Compact_Disc_subcode
[karafun]: http://www.karafun.com/karaokeplayer/
