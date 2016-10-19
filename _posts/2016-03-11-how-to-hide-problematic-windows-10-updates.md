---
layout: post
title: "如何隐藏有问题的 Windows 10 更新"
quote: "真是应了那句话：微软的怪毛病，还得靠微软隐藏的工具解决"
date: "2016-03-11 08:03:44 +0800"
slug: how-to-hide-problematic-windows-10-updates
author: James Shih
categories:
  - PC
tags:
  - Windows 10
---
微软大力推广的 Windows 10，是一个快速更新的操作系统，把用户当 QA 使。最近更是连续更新出包（[其一][1]、[其二][2]、[其三][3]）。再加上 Windows 10 移除了关闭或者隐藏更新的功能，用户只能眼睁睁看着有问题的更新被卸掉了又被自动重新安装。

真是应了那句话：微软的怪毛病，还得靠微软隐藏的工具解决（[即视感][4]）。事实上，微软自己也对更新缺乏自信，所以它[还是提供了一个疑难解答工具（KB3073930）][5]，专门用来显示或隐藏更新……

<figure>
  <img src="/media/2016/2016-03-11-how-to-hide-problematic-windows-10-updates/wushowhide-diagcab.png" srcset="/media/2016/2016-03-11-how-to-hide-problematic-windows-10-updates/wushowhide-diagcab.png 2x" alt="wushowhide-diagcab">
  <figcaption>显示或隐藏更新疑难解答</figcaption>
</figure>

选择 _隐藏更新_（Hide updates）选项，系统会首先检查目前可以安装的更新，然后只需要在列表中勾选需要隐藏的更新并确认即可。这些更新将暂时不会被安装，也不能在设置 App 中看到。

显然，没有改好的 Bug 仍然是 Bug，你迟早还是需要更新。当微软终于推出了补丁的补丁可以一起服用的时候，你会想要重新显示这些更新。这时候仍然需要用这个工具，选择 _显示隐藏的更新_（Show hidden updates）。

[1]: http://news.softpedia.com/news/windows-10-users-upset-with-microsoft-resetting-default-apps-over-and-over-again-496747.shtml
[2]: http://news.softpedia.com/news/how-to-fix-issues-caused-by-windows-10-cumulative-update-kb3140743-501298.shtml
[3]: http://news.softpedia.com/news/it-might-happen-again-windows-10-cumulative-update-kb3140768-fails-to-install-501522.shtml
[4]: /2010/09/10/%E2%80%9C%E6%97%A0%E6%B3%95%E6%89%93%E5%BC%80outlook%E7%AA%97%E5%8F%A3%E2%80%9D%E7%9A%84%E9%97%AE%E9%A2%98/
[5]: https://support.microsoft.com/en-us/kb/3073930
