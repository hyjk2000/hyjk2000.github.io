---
title: 用正则表达式验证或提取URL
author: James Shih
layout: post
permalink: /2010/12/06/parsing-url-with-regular-expression/
jabber_published:
  - 1291643073
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"13298777";s:7:"blog_id";s:8:"12911913";s:9:"mod_stamp";s:19:"2010-12-07 01:51:17";}'
categories:
  - PHP
  - Web 开发
tags:
  - RegEx
  - URL
  - 正则表达式
---
一个完整的URL，可能会包含以下部分：

<span style="color: #00ff00;">http://</span><span style="color: #0000ff;">www.website.com</span><span style="color: #4bacc6;">:8080</span><span style="color: #ffc000;">/lab/sub/</span><span style="color: #ff0000;">page.php</span><span style="color: #9b00d3;">?do=list&type=2</span><span style="color: #d16349;">#tool</span>

我试着写了一个能识别并捕获URL中这些部分的正则表达式。

/^<span style="color: #00ff00;">((?:http[s]?|ftp):\/\/)?</span><span style="color: #0000ff;">([^:\/\s]+|\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})</span><span style="color: #4bacc6;">(:\d+)?</span>(?:<span style="color: #ffc000;">((?:\/\w+)*\/)</span><span style="color: #ff0000;">([\w\-\.]+[^#?\s]+)?</span><span style="color: #9b00d3;">(\?[^\s#]*)?</span><span style="color: #d16349;">(#\w+)?</span>)?$/i

这样用不同的颜色标记，就很容易看清了。

可以在<a href="http://rubular.com/r/rpNtEU0YOZ" target="_blank">这里</a>试一试。这个正则表达式能干净利落地捕获URL的各个部分。

另外，在PHP里可以用<span style="font: 10pt consolas,monotype;">parse_url()</span>来分析URL。如果URL明显不合法，该函数返回<span style="font: 10pt consolas,monotype;">FALSE。</span>