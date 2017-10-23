---
title: Mac Finder 显示隐藏文件
author: James Shih
layout: post
permalink: /2012/12/26/mac-finder-show-hidden-files/
categories:
  - Apple
  - Mac
  - OS X
tags:
  - Automator
  - Finder
  - Mac
  - OS X
  - Shell Script
---
<figure>
  <img src="/media/legacy/2012/12/20121226_Mac_Finder_Show_Hidden_Files_2.png" alt="20121226_Mac_Finder_Show_Hidden_Files_2">
</figure>

Mac 的 Finder 默认是不显示隐藏文件的，而且在 Finder 的偏好设置中并没有提供相应的设置项。不过，Finder 是可以显示隐藏文件的，只不过你需要通过终端来输入命令进行设置。

打开 **Launchpad**，在 **其他** 中打开 **终端**，输入以下命令：

```bash
defaults write com.apple.finder AppleShowAllFiles TRUE
killall Finder
```

注意，这会重新启动 Finder，所以请不要在有文件操作正在进行时执行这些命令。

现在，Finder 中就会显示出隐藏文件了。不过，为了安全和整洁起见，最好还是隐藏它们，只要把以上命令中第一行的 TRUE 改为 FALSE 即可。

说到这里，也许你会觉得每次切换显示隐藏文件都得在终端里输入命令，太麻烦。其实，我们可以用 Mac 附带的 Automator 来一劳永逸地解决这个问题。

<!--more-->

<a class="btn" href="/media/legacy/2012/12/显示隐藏文件.workflow.zip">显示隐藏文件.workflow.zip</a>

下载以上文件解压，将 **显示隐藏文件.workflow** 移动（按 <kbd title="Command + C">⌘C</kbd> 和 <kbd title="Option + Command + V">⌥⌘V</kbd> 来移动）到 **~/Library/Services** 目录（在 Finder 中按 <kbd title="Shift + Command + G">⇧⌘G</kbd>，输入上述路径来直接打开此目录）。最后在终端中使用 **killall Finder** 重新启动 Finder 即可。

以后每次需要切换显示隐藏文件时，只要在 Finder 菜单的 **服务** 下选择 **显示隐藏文件** 即可。

这个 Automator 工作流的原理就是执行了一个 Shell 脚本，读取当前 com.apple.finder AppleShowAllFiles 的值，如果为 FALSE 则设置为 TRUE，反之亦然。

```bash
if [ `defaults read com.apple.finder AppleShowAllFiles` != "TRUE" ]
then
	defaults write com.apple.finder AppleShowAllFiles TRUE
else
	defaults write com.apple.finder AppleShowAllFiles FALSE
fi
killall Finder
```

在 Automator 中新建一个 **服务**，按下图设置并保存即可：

<figure>
  <img src="/media/legacy/2012/12/20121226_Mac_Finder_Show_Hidden_Files_1.png" alt="20121226_Mac_Finder_Show_Hidden_Files_1">
</figure>
