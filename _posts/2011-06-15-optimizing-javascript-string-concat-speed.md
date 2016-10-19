---
title: 如何在 JavaScript 中高效地连接字符串
author: James Shih
layout: post
permalink: /2011/06/15/optimizing-javascript-string-concat-speed/
jabber_published:
  - 1308130831
authorsure_include_css:
  - 
categories:
  - JavaScript
  - Web 开发
tags:
  - JavaScript
  - 优化
  - 字符串连接
---
字符串连接一直是 JavaScript 中最慢的操作之一。在 JavaScript 中连接字符串有两种常用做法：一是通过加法运算符（+）；二是将字符串碎片放入 Array，再调用 Array 对象的 join() 方法。

关于这两种方法孰优孰劣，一直有很多争论。早期的浏览器没有对用加法运算符进行字符串连接进行优化。由于字符串是不可变的，进行连接操作时必然要创建中间字符串来储存连接结果，而在后台大量进行这种操作会使字符串连接的性能极其低下。于是，大部分人认为利用 Array 对象的性能更好。

真相的确如此吗？

<!--more-->

于是我决定做一个简单粗略的测试。让浏览器连接一百万个字符串，并记录下它们要花掉多少时间。为了获得更全面的结果，测试分成短字符串连接和长字符串连接两种，并且每种测量三次取平均值进行记录。

获取测试程序：<a class="btn" href="http://jsfiddle.net/hyjk2000/GpYUG/" target="_blank">Demo</a>

**Update (08/06/2013): **jsPerf.com 上的测试实例：<a class="btn" href="http://jsperf.com/string-concatenation-performance-test" target="_blank">Test Case</a>

由于 IE6 的性能实在太差，用加法运算符连接十万个字符串就会假死，于是只进行了连接 50000 个字符串的操作。另外，因为测试条件所限，IE6 是在 Windows XP Mode 中执行的，IE8 是在另一台电脑上执行的。

首先是一百万个短字符串（&#8217;dummy&#8217;）连接测试结果：

<img src="/media/legacy/2011/06/20110615_string_concat_speed_1.png" alt="20110615_string_concat_speed_1">

从结果上可以明显看出，在 Chrome 12、Firefox 4、IE 8 上，用加法运算符的性能要高得多。尤其是 Firefox 4，两种方法的速度差别相当惊人。这是因为这些浏览器已经优化了字符串连接，在所有情况下用数组技术都要比用加法运算符慢。

IE 6 不出意外地出现了相反的情况，使用数组技术有非常大的性能优势。IE 7 的情况与 IE 6 基本相同。

IE 9 的结果比较奇怪，我推测可能是因为 IE 9 的数组处理性能有了很大改进，所以在后面我加大了字符串长度后，在 IE 9 上使用数组技术就比用加法运算符慢了。

然后是一百万个长字符串（&#8217;thequickbrownfoxjumpsoverthelazydog195476825412355849751364859&#8242;）的连接测试结果：

<img src="/media/legacy/2011/06/20110615_string_concat_speed_2.png" alt="20110615_string_concat_speed_2">

随着字符串的长度增长，在 Chrome 12、Firefox 4、IE 8、IE 9 上，使用加法运算符所消耗的时间并没有显著增长，但是使用数组技术所消耗的时间有一定增长。

IE 6 仍然呈现了数组技术的优势，用加法运算符甚至出现了假死，这是因为处理中间结果所消耗的资源随着字符串长度的增长而呈指数增长。

现在我们就得到了问题的答案：利用数组技术来连接字符串性能更高的说法已经不再适用于现代浏览器。不过，仍然有很多用户还在坚持使用老版本的浏览器（参见<a href="http://www.theie6countdown.com/" target="_blank">此处</a>）。所以还是应该结合用户群的特点来选择适当的算法。不过在大部分情况下，用加法运算符是更佳的选择。
