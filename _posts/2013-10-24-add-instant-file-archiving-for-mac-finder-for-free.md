---
title: 为 Mac Finder 增加右键文件打包压缩（免费）
quote: 有没有最简单、免费的方法，给我的 Mac 加个右键压缩？当然，那就是 7-Zip 的 Mac 移植版 —— p7zip。
author: James Shih
layout: post
permalink: /2013/10/24/add-instant-file-archiving-for-mac-finder-for-free/
authorsure_include_css:
  -
categories:
  - Apple
  - OS X
tags:
  - 7-Zip
  - Automator
  - Homebrew
  - OS X
---
<figure>
  <img src="/media/legacy/2013/10/20131024_add-instant-file-archiving-for-mac-finder-for-free_1.png" alt="20131024_add-instant-file-archiving-for-mac-finder-for-free_1">
</figure>

在 Windows 上用惯了 7-Zip 和 WinRAR，来到 Mac 却突然发现没有类似的工具？Mac 自带的 Zip 工具确实让人吐糟无力，压缩率低就不说了，因为 Mac 上文件名是 Unicode 编码，到了 GBK 编码的 Windows 上解压了文件名全是乱码有没有？Mac 上的隐藏文件（.DS_Store，.Spotlight-V100之类）每次都得删很麻烦有没有？

总之，用起来很憋屈。尽管有用起来不憋屈的压缩软件可以用（比如 [Entropy][1]），但是其 ￥123 的价格有点坑爹，而且它的许多功能其实用不上。

有没有最简单、免费的方法，给我的 Mac 加个右键压缩？当然，那就是 7-Zip 的 Mac 移植版 —— [p7zip][2]。

<!--more-->

等等……p7zip 好像是命令行工具？我每次压缩个文件还得打开终端敲命令么？当然不是，虽然一开始安装的时候你还是得敲几条命令，但往后你用它的时候就不用再敲一个字了。这就是一劳永逸。

另外，Windows 上的 WinRAR 可以完美支持 7-Zip 压缩格式，而且 7-Zip 可以很好地处理文件名编码的问题，压缩率和压缩/解压缩速度也比 WinRAR 高。另外，7-Zip 是开源的压缩格式，而 WinRAR 是商业授权的。所以现在看来，除了 Windows 上的习惯，并没有理由继续使用 WinRAR。

### 安装 p7zip

因为 p7zip 没有发布 Mac 上的二进制版本，只有源码包。所以，你需要在你的 Mac 上编译安装。别被吓到了，有了 [Homebrew][3]，一切都很简单。Homebrew 是 Mac 上的一款包管理器，用它可以很方便地下载很多开源命令行工具的源码包，并编译安装到你的 Mac 上。有了它，再加上一点点面对命令行的勇气，你就可以不花一分钱让你的 Mac 做更多的事情。

打开终端，输入这条命令回车，即可安装 Homebrew：

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

接着安装 p7zip：

```bash
brew install p7zip
```

好了，现在你可以用 7za 命令了：

```bash
7za a -m0=LZMA2 -r -x\!.* archive.7z [StuffToArchive]
```

对命令参数的解释：

`a`
:   添加到压缩包

`-m0=LZMA2`
:   压缩算法及选项，这里指定使用速度又快压缩率又高的 LZMA2 算法

`-r`
:   包括子目录及内容

`-x\!.*`
:   排除 Mac 下的隐藏文件，这里惊叹号代表使用通配符，因为惊叹号本身和命令行有冲突所以加了一个反斜杠转义

`archive.7z`
:   要创建的压缩文件的文件名

`[SruffToArchive]`
:   要压缩的文件/文件夹，可以输入多个

更多的 p7zip 命令用法，可以输入 `7za --help`。

### 添加到 Finder 右键菜单

下一步就是把 p7zip 添加到 Finder 的右键菜单。这一步是通过 Mac 自带的 [Automator][4] 完成的。用 Automator 可以为 Finder 制作一个服务，这个服务可以获取你在 Finder 中选择的文件，用 p7zip 压缩它们。

下载以下文件解压，然后把 **添加到 7-Zip 压缩文件.workflow** 放进 **~/Library/Services** 目录。现在你可以在 Finder 中选择一些文件，点击鼠标右键看看。（如果相应的选项没有出现，你可能需要重新启动 Finder，在终端中输入 `killall Finder`）

<a class="btn" href="/media/legacy/2013/10/%E6%B7%BB%E5%8A%A0%E5%88%B0%207-Zip%20%E5%8E%8B%E7%BC%A9%E6%96%87%E4%BB%B6.zip">添加到 7-Zip 压缩文件.zip</a>

有兴趣的话，你可以自己试着在 Automator 里制作这个服务。

<figure>
  <img src="/media/legacy/2013/10/20131024_add-instant-file-archiving-for-mac-finder-for-free_2.png" alt="20131024_add-instant-file-archiving-for-mac-finder-for-free_2">
</figure>

### 解压工具

至于解压工具，我推荐 [The Unarchiver][5]。这是个免费的，非常简单的工具，用法和 Mac 自带的解压工具一样。

 [1]: https://itunes.apple.com/cn/app/entropy/id437151949
 [2]: http://sourceforge.net/projects/p7zip/
 [3]: http://brew.sh/
 [4]: http://www.macosxautomation.com/automator/
 [5]: https://itunes.apple.com/cn/app/the-unarchiver/id425424353
