---
title: 使用 Google CDN 的技巧
author: James Shih
layout: post
permalink: /2011/09/05/google-cdn-tips/
categories:
  - JavaScript
  - Web 开发
tags:
  - CDN
  - CDN Fallback
  - jQuery
  - Tips
  - 技巧
---
Google 慷慨地提供了 CDN，我们现在都喜欢从 Google CDN 上载入 jQuery 等等著名的 JavaScript API。

使用 Google CDN 有以下几点好处：

1.  速度快：Google 强大的服务器和网络能保证更快的响应和下载速度
2.  可以并行下载：浏览器在下载 JavaScript 的时候就不愿意同时下载其他东西了，除非这个 JavaScript 和其他东西不在同一个域
3.  跨站缓存：如果用户之前浏览过使用 Google CDN 的网站，他们在浏览你的网站时就可以直接调用缓存中的 JavaScript，而不必重新下载

但是，Google CDN 也并不是 100% 可靠，尤其是在中国；而且，如果你用了 HTTPS，浏览器通常会拒绝载入非加密连接的内容。这两种情况都会造成载入失败，你的页面会变得一团糟。

所以，要更好地利用 Google CDN，还有一些小技巧。

<!--more-->

首先，**一般来说，在网页中调用其他文件时，都应该在 URL 中省略协议名称**，就像这样：

```html
<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.6.2/jquery.min.js"></script>
```

这样一来，如果用户正以 HTTPS 协议浏览你的网站，浏览器会自动通过 HTTPS 协议来载入 jQuery，你不用再为协议的问题操心。而且这是[完全合法的 URL][1]，连远古浏览器（像 IE 3 之类）都支持。

也不要在地址中使用 https://，因为 HTTPS 协议不会缓存任何东西，也不会使用缓存中的内容，你的用户将无法享受跨站缓存带来的好处。

其次，**你应该让 Google CDN 载入失败时，自动载入你服务器上的副本**，就像这样：

```html
<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jqueryui/1.8.16/jquery-ui.min.js"></script>
<script type="text/javascript">!window.jQuery.ui && document.write(unescape('%3Cscript src="/script/jq/jquery-ui.min.js?1.8.16" type="text/javascript"%3E%3C/script%3E'));</script>
```

这就是 CDN Fallback。这样一来，一旦用户无法连接 Google CDN，而且缓存中没有从 Google CDN 下载的 jQuery 库，就会自动从你的服务器下载，不会影响到网站的运行。

 [1]: http://tools.ietf.org/html/rfc3986#section-4.2