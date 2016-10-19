---
title: 在 Mac 上更方便地使用 JSPacker
author: James Shih
layout: post
permalink: /2013/04/10/instant-jspacker-on-mac/
categories:
  - JavaScript
  - Linux
  - Mac
  - Web 开发
tags:
  - JSPacker
  - Shell Script
  - Shell 脚本
  - 技巧
---
<figure>
  <img src="/media/legacy/2013/04/20130410_instant_jspacker_on_mac_2.png" alt="20130410_instant_jspacker_on_mac_2">
</figure>

[JSPacker][1] （作者是 Dean Edwards）对于前端开发人员来说再熟悉不过了，它是典型的 JavaScript 压缩与混淆工具。它不仅可以减小 js 文件的大小，还可以加密其内容。不过，JSPacker 在平时使用中也会遇到一些麻烦。比如，需要把代码复制到网页上，处理完后在复制下来，操作比较繁琐；此外，处理后的代码执行时常常会报错，原因时 JSPacker 对代码中一些不太规范的写法很敏感，容易处理错误。

为了更好地利用 JSPacker，我们可以用其网站上提供的 Perl 版程序（作者是 Rob Seiler）在本地运行。为了预防处理后无法运行的问题，我们可以用另一款 JavaScript 压缩工具 [YUI Compressor][2] 先整理一遍，修改容易引起问题的语法结构，然后再交给 JSPacker 处理。我们可以编写一个 Shell 脚本自动运行以上过程。最后，利用 Mac 自带的 Automator 制作一个 Finder 服务，这样就可以在 Finder 中右键单击 js 文件选择 JSPack 直接压缩了。

<!--more-->

JSPacker Perl 版可以在[这里][3]获得，Mac 上已安装好了 Perl，无需再装。

YUI Compressor 是 Java 程序，需要在 Mac 上安装 Java，在[这里][4]下载。

准备好后就可以开始干活了。首先把 JSPacker Perl 压缩包中的 jsPacker.pl、Pack.pm、ParseMaster.pm，和 YUI Compressor 压缩包中的 yuicompressor-2.4.7.jar 解压出来放在同一目录。

接着，因为 jsPacker.pl 调用了另外两个 pm 文件，而我们不想把这两个文件放到 Perl 的包含目录（@INC）下，所以我们需要修改 jsPacker.pl，在顶部添加如下两行：

```perl
#!perl
#jsPacker (July 2005)
#
use FindBin;
use lib "$FindBin::Bin/.";
```

这样 jsPacker.pl 就会直接调用同目录下的两个 pm 文件。

下一步，编写 Shell 脚本 jspacker.sh：

```bash
#!/bin/bash
JSPACKER_PATH=$(dirname "$0")
WORK_PATH=$(dirname "$1")
filename=$(basename "$1")
extension="${filename##*.}"
filename="${filename%.*}"
cd "$WORK_PATH"
java -jar "$JSPACKER_PATH/yuicompressor-2.4.7.jar" $1 -o "$filename.yui.$extension"
perl "$JSPACKER_PATH/jsPacker.pl" -i "$filename.yui.$extension" -o "$filename.min.$extension" -e62 -f -q
rm -rf "$filename.yui.$extension"
```

其中，第 4 － 5 行获取待转换文件的文件名和扩展名，采用了 Shell Parameter Expansion，更多的写法可以参考[这里][5]。

通过执行这个脚本，就可以压缩 js 文件了：

```bash
$ ./jspacker.sh some.js
$ ls *.js
some.js     some.min.js
```

最后一步，用 Automator 制作一个 Finder 服务来调用这段脚本，这样就可以做到在 Finder 中右键点击 js 文件选择 JSPack 来直接压缩了：

<figure>
  <img src="/media/legacy/2013/04/20130410_instant_jspacker_on_mac_1.png" alt="20130410_instant_jspacker_on_mac_1">
</figure>

当然，如果你很忙或者懒得弄，可以直接下载我做好的文件，解压到 **~/Downloads** 目录，将 **JSPack.workflow** 文件拷贝到 **~/Library/Services** 目录就行了（你可能需要在终端中使用 **killall Finder** 重新启动 Finder）：

<a class="btn" href="/media/legacy/2013/04/JSPack.zip">JSPack.zip</a>

 [1]: http://dean.edwards.name/packer/2/
 [2]: http://yui.github.com/yuicompressor/
 [3]: http://dean.edwards.name/download/#packer
 [4]: http://java.com/
 [5]: http://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html#Shell-Parameter-Expansion
