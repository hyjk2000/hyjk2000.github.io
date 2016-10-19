---
title: CSS 透明
author: James Shih
layout: post
permalink: /2011/12/15/css-opacity/
categories:
  - HTML/CSS
  - Web 开发
tags:
  - -ms-filter
  - CSS
  - filter
  - opacity
  - 浏览器兼容
  - 透明
---
以下是一个能够同时兼容 Chrome、Firefox 等现代浏览器和 IE5.5-8 及 IE8 兼容模式的 CSS 透明写法：

<!--more-->

```css
.translucent {
     opacity: .5; /* for modern browsers */
     -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(opacity=50)"; /* for IE8 */
     filter: alpha(opacity=50); /* for IE */
}
```

注意 -ms-filter 和 filter 的顺序，如果颠倒，则 IE8 兼容模式下会失效。

<a class="btn" target="_blank" href="http://jsfiddle.net/hyjk2000/xksgD/embedded/result/">Demo</a>