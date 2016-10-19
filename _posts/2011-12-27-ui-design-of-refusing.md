---
title: 拒绝邀请的 UI 设计
author: James Shih
layout: post
permalink: /2011/12/27/ui-design-of-refusing/
categories:
  - UX
  - Web 开发
tags:
  - design
  - social network
  - usability
  - UX
  - 可用性
  - 社交网络
  - 设计
---
<img src="/media/legacy/2011/12/20111227_ui_design_of_refusing_1.jpg" alt="20111227_ui_design_of_refusing_1">

在社交网络或客户端中，都会有很多邀请或类似的动作，由用户决定是否接受。一般来讲，如果收到了不合意的邀请，人们大都希望在拒绝时能够温和一点。也就是说，“忽略”要好过明言拒绝。不过当遇到恶意的大量邀请时，人们同样可以将恶意用户屏蔽。这样的设计能够很好地模拟出生活中人们进行社交活动的习惯。

不过，如果像上图这样简单地给出一个 modal dialog，就会造成一个问题：用户可能不愿意接受邀请，但又不好明言拒绝，所以点击“拒绝”按钮时会犹豫。

让用户犹豫的设计不是好设计。

<!--more-->

稍微好一点的设计是，给这个 modal dialog 加一个关闭按钮。这样一来，用户就可以通过关闭对话框来忽略掉这个请求。QQ 的好友验证请求的设计和这个很类似。

<img src="/media/legacy/2011/12/20111227_ui_design_of_refusing_2.jpg" alt="20111227_ui_design_of_refusing_2">

当然，在我看来这确实还不够完美。如果“拒绝”按钮的作用与关闭按钮一样，为何不明确地告诉用户，让用户放心地点击呢？那个又大又漂亮的按钮，是不是比一个小小的“×”更容易点击呢？当然，如果系统检测到多次恶意邀请的话，再加上一个复选框“将此用户加入黑名单”就行了。Google+ 采用了类似的设计。

<img src="/media/legacy/2011/12/20111227_ui_design_of_refusing_3.jpg" alt="20111227_ui_design_of_refusing_3">

不过，这个设计也不是放在哪个项目中都合适。因为对于有些目标用户群，有时也需要明言的拒绝<del>，比如相亲网站（想象一下凤姐邀请你时 XD）</del>。这时，与其让用户猜来猜去，不如在 UI 上写明，按哪一个按钮是明言拒绝，哪一个是忽略。

总之，就是要让用户更容易明白，不会出现犹豫。
