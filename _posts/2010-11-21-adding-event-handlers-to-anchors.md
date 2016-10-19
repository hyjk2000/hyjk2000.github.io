---
title: 为链接添加事件处理
author: James Shih
layout: post
permalink: /2010/11/21/adding-event-handlers-to-anchors/
jabber_published:
  - 1290315942
categories:
  - JavaScript
  - Web 开发
tags:
  - anchor
  - event handling
  - 事件处理
  - 浏览器兼容
  - 链接
---
在Web开发过程中，肯定需要让用户调用一些JavaScript函数。一般这些函数放在按钮上（`<input type="buton">` 或者 `<button>`），但少数情况下需要放在一些链接（`<a>`）上。常用的做法有以下几种：

```html
<a href="javascript:fnSomeFunc()">Foobar</a>
<!-- 客户端状态栏会显示出函数名 -->

<a href="#" onclick="fnSomeFunc()">Foobar</a>
<!-- 函数名被隐藏，可是单击后总会跳到页面顶部 -->

<a name="anchor" href="#anchor" onclick="fnSomeFunc()">Foobar</a>
<!-- 单击后不会跳到页面顶部，但地址后面会加上#anchor，而且这个方法看起来十分蛋疼 -->

<a href="javascript:void(0);" onclick="fnSomeFunc()">Foobar</a>
<!-- 似乎很完美的解决方法，可是IE会误认为你转到了另一个页面 -->
```

最后一种方法我看到不少人在用，但是它有个小问题：

在一些巨大的表单中，为了防止用户不慎丢失填好的数据，经常使用window.onbeforeunload属性来添加一些离开页面时的确认信息。

```javascript
window.onbeforeunload = function() { return 'Are you sure?'; };
```

这个函数返回的字符串将显示给用户，如下图所示：

![ie-confirm](/media/legacy/2010/11/image.png)

当用户单击这个链接的时候，因为href里不是本页上的锚点，IE会认为你转到了另一个页面。于是上图那个对话框就弹出来了，把用户吓个半死。在用第4种方法的时候，JS函数会先执行，然后对话框才弹出来。更糟糕的是如果你用的是第1种方法，对话框会先弹出来。这时用户因为害怕辛辛苦苦填写的表单全部丢失，多半会选择“留在此页上”，JS函数就根本不会执行。如果这个问题多出现几次，用户多半会直接按下浏览器上的“X”。

以上这几个方法都有些问题，只能随机应变了。当不使用 `window.onbeforeunload` 的时候，可以使用方法1和方法4；当需要使用 `window.onbeforeunload` 时，就只好用第3种蛋疼法了。或者，干脆用 `<button>` 好了。

```html
<button onclick="fnSomeFunc()">Foobar</button>
```

这样多干净多方便！
