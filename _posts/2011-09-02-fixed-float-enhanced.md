---
title: '供用户参考用的页面漂浮元素 &#8211; 加强版'
author: James Shih
layout: post
permalink: /2011/09/02/fixed-float-enhanced/
categories:
  - HTML/CSS
  - JavaScript
  - Web 开发
tags:
  - fixed float
  - HTML
  - JavaScript
  - jQuery
  - jQuery Plugin
---
在以前的[这篇文章][1]中，我提到 jqueryfordesigners.com 上有一个对 Apple Store 上的漂浮元素的实现研究。这个方法确实能做到和 Apple Store 一样的效果，不过却只完成了一半的工作。因为用户如果更改了浏览器窗口大小，漂浮元素的水平定位却不会更新，导致漂浮元素的位置错误。

为了修复这个问题，我为这个实现方法添加了水平定位更新的动作。

<!--more-->

为了取得 viewport 宽度变化后漂浮元素应处的位置，以原漂浮元素所在位置与窗口中心的距离，和现在窗口的宽度，来计算出新的漂浮元素与窗口左侧的距离。

首先，获取元素与窗口中心的距离：

```javascript
var x = this.position().left;
var distanceFromCenter = $(window).width() / 2 - x; // 漂浮元素在窗口中心左侧时为正值; 在右侧时为负值
```

然后，在浏览器的 onresize 事件中加入以下操作：

```javascript
// 取得新的窗口宽度
var newHalfViewportWidth = $(window).width() / 2;

// 如果窗口宽度太小
if (newHalfViewportWidth < distanceFromCenter) {
    // 漂浮元素在窗口中心左侧时为正值，这时漂浮元素应该紧靠浏览器窗口左侧，不应被挤出浏览器窗口左侧
    x = 0;
} else {
    // 漂浮元素在窗口中心右侧时为负值，则 newHalfViewportWidth 肯定不会小于 distanceFromCenter，这时漂浮元素应被挤出浏览器窗口右侧
    x = newHalfViewportWidth - distanceFromCenter;
}

// 最后，更新漂浮元素的水平定位
$meWrapper.css('left', x + 'px');
```

我已将这个比较完整的实现做成 jQuery 插件，请参见：

<a class="btn" href="http://hyjk2000.github.io/jquery.sticky-float/">Demo</a>

**UPDATE(10/26/2011): **

事实上，要获取漂浮元素新的横向坐标，还有更简单粗暴的方法：就是先取消浮动，获取坐标，然后再让它漂浮起来。就像这样：

```javascript
$(window).resize(function () {
    // 取消浮动
    _me.removeAttr('style');

    // 获取横向坐标
    x = _me.offset().left;

    // 再设置漂浮
    var y = $(this).scrollTop();
    if (y >= _top) _me.attr('style', 'position:fixed;top:0;left:' + x + 'px');
});
```

 [1]: /2010/06/18/%E4%BE%9B%E7%94%A8%E6%88%B7%E5%8F%82%E8%80%83%E7%94%A8%E7%9A%84%E9%A1%B5%E9%9D%A2%E6%BC%82%E6%B5%AE%E5%85%83%E7%B4%A0/
