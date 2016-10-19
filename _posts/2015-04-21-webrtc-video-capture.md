---
title: WebRTC 之视频捕获
quote: WebRTC 的意思是 Web 实时通讯，通过实现点对点连接，让 Web 通讯的时延降到最低，而目前 WebRTC 最吸引人的应用是实时视频通讯
author: James Shih
layout: post
permalink: /2015/04/21/webrtc-video-capture/
categories:
  - Web 开发
tags:
  - WebRTC
  - HTML5
  - JavaScript
---
## 什么是 WebRTC

[WebRTC][wiki-webrtc]（Web Real-Time Communication）是实现浏览器之间点对点实时通讯的一套技术规范（现在也支持 iOS 和 Android 应用）。2010 年 5 月，Google 收购了 VoIP 开发商 Global IP Solutions，在其技术基础上开发了 WebRTC，并于一年后开源。目前，WebRTC 1.0 是 [W3C 的标准草案][w3cwd-webrtc]，Chrome 30+ 和 Firefox 36+ 实现了[对 WebRTC 的支持][caniuse-webrtc]。

以往，浏览器间的通讯绝大部分还必须要通过服务器中转来间接实现，这需要浏览器和服务器间进行全双工通讯。尽管我们有老的技术（如轮询、[Comet][wiki-comet]），也有新技术（如 [WebSocket][wiki-websocket]）来实现，但它们都不是为浏览器间直接通讯而设计的。这些通过服务器中转的方式不可避免地带来了明显的开销，通讯的时延和速度不理想，数据的私密性也难以保证。而 WebRTC 就是专门解决这个问题的。

利用 WebRTC 可以在客户端之间传送任意数据，但它目前最吸引人的应用还是实时视频通讯。多年来，这样的应用都是通过浏览器插件（如 Flash 和 Silverlight）来实现，但 HTML5 的崛起改变了一切，新的 JavaScript API，极大增强了前端代码对硬件的控制能力：GPS、加速度计、陀螺仪、电池、GPU、视频和音频……等等。为了实现视频通讯，我们需要用 `getUserMedia`，也就是 Stream API，来获取设备的视频和音频设备，具体来讲，就是摄像头和麦克风。目前，只要支持 WebRTC 的浏览器都[支持 Stream API][caniuse-stream]。

WebRTC 系列文章将一步步讲述一个实时通讯应用是怎样实现的，而本文的内容就是第一步：捕获视频。在本例中，我们将获取摄像头的视频数据，然后显示在页面上一个 `<video>` 标签中。

## 简单的开始

首先，我们在页面上放一个 `<video>` 标签：

```html
<video class="local" autoplay></video>
```

然后，调用 `getUserMedia`，它的三个参数如下：

<dl>
  <dt><code>constraints</code></dt>
  <dd>指定媒体规格（如视频分辨率、帧速率等），Chrome 和 Firefox 上有所不同，见后文</dd>
  <dt><code>successCallback</code></dt>
  <dd>成功后的回调函数，返回一个 `MediaStream` 媒体流对象作为参数</dd>
  <dt><code>errorCallback</code></dt>
  <dd>失败后的回调函数</dd>
</dl>

获取到媒体流对象后，该怎样把它交给 `<video>` 标签呢？答案是[对象 URL][mdn-object-url]，对象 URL 可以代表某一个 File 对象或 Blob 对象，而我们拿到的 `MediaStream` 对象就是一个 Blob 对象。利用 `URL.createObjectURL` 方法来创建 `MediaStream` 对象的 URL，并填入 `<video>` 标签的 `src` 属性就行了。

```javascript
var localVideo        = document.querySelector("video.local");
var localMediaStream  = null;
var localMediaURL     = null;

function getUserMedia(callback) {
  if (navigator.getUserMedia === undefined) {
    navigator.getUserMedia = navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
    if (navigator.getUserMedia === undefined) {
      console.error('getUserMedia not supported.')
      return false;
    }
  }
  navigator.getUserMedia({ video: true, audio: true }, callback, function(e) {
    console.error('getUserMedia failed: ' + e.message);
  });
}

getUserMedia(function(stream) {
  localMediaStream = stream;
  localVideo.src = localMediaURL = URL.createObjectURL(localMediaStream);
});
```

在有摄像头的电脑上，用 Chrome 或 Firefox 打开这个页面。浏览器会询问你是否允许页面访问你的摄像头，点击允许后，页面上就会显示出视频画面。

## 媒体规格

以上代码中，我们只是简单地指定媒体规格为 `{ video: true, audio: true }`，要求浏览器返回视频和音频，而对具体规格没有要求。事实上，如果你摄像头能输出 720p 甚至 1080p 的视频，用以上代码也只能获取到 VGA 规格。所以我们需要对规格做进一步设置。

不幸的是，目前 Chrome 上的实现 `webkitGetUserMedia` 支持的规格格式遵循一个[早期的规范][constraints-chrome]，而不是[最新的规范][constraints-firefox]。所以，我们得先判断浏览器类型，再予以指定：

```javascript
function getMediaConstraints() {
  if (navigator.webkitGetUserMedia) {
    // Chrome
    return {
      video: {
        mandatory: { minWidth: 640, minHeight: 480 },
        optional: [{ minWidth: 1280 }, { minHeight: 720 }, { facingMode: "user" }]
      },
      audio: true
    };
  }
  // Firefox
  return {
    video: {
      width: { min: 640, ideal: 1280 },
      height: { min: 480, ideal: 720 },
      facingMode: "user"
    },
    audio: true
  };
}
```

使用以上的规格，就可以在摄像头支持时使用 720p 格式，若不支持则回落到 VGA 格式。如果是在手机等移动设备上，则优先选择自拍摄像头。Firefox 38 以前的版本支持也不完全符合标准，不支持 `ideal` 参数，所以只会返回 VGA 格式。使用这些规格的时候需要谨慎，因为如果设备连你指定的最低标准都达不到，`getUserMedia` 会失败。

## 一些润色

你也许在尝试以上例子的时候，会感到有些不对劲，跟平时使用其他聊天软件有点不同。事实上，大部分聊天软件里，你看到自己的画面是左右翻转的，就像镜子一样。因为人们大都习惯照镜子，所以当你看到没有翻转的视频时会觉得别扭，想捋一捋左边的头发却伸出了右手。

其实这个翻转很容易实现，用 CSS3 Transform 就行了：

```scss
video {
  &.local {
    transform: rotateY(180deg);
  }
}
```

你也可以用 CSS3 Filter 给视频添加滤镜效果：

```scss
video {
  &.local {
    transform: rotateY(180deg);
  }
  &.grayscale {
    filter: grayscale(1);
  }
  &.sepia {
    filter: sepia(1);
  }
  &.blur {
    filter: blur(5px);
  }
  &.invert {
    filter: invert(.8);
  }
  &.inkwell {
    filter: grayscale(1) brightness(0.45) contrast(1.05);
  }
}
```

## 完整例子

{% gist hyjk2000/10e0bffa19181fc3f600 %}

## 下期预告

[WebRTC 之点对点连接][webrtc-peer-connection]将会介绍如何在浏览器之间建立点对点连接。


[wiki-webrtc]: https://en.wikipedia.org/wiki/WebRTC
[w3cwd-webrtc]: http://www.w3.org/TR/webrtc/
[caniuse-webrtc]: http://caniuse.com/#feat=rtcpeerconnection
[wiki-comet]: https://en.wikipedia.org/wiki/Comet_(programming)
[wiki-websocket]: https://en.wikipedia.org/wiki/WebSocket
[caniuse-stream]: http://caniuse.com/#feat=stream
[mdn-object-url]: https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL
[constraints-chrome]: http://tools.ietf.org/html/draft-alvestrand-constraints-resolution-00
[constraints-firefox]: http://w3c.github.io/mediacapture-main/getusermedia.html#mediastreamconstraints
[webrtc-peer-connection]: /2015/05/16/webrtc-peer-connection/
