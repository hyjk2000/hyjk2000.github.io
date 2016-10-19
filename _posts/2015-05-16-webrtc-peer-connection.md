---
title: WebRTC 之点对点连接
quote: WebRTC 相对于 WebSocket 等技术最大的优势，也就是它存在的根本，是实现点对点连接
author: James Shih
layout: post
permalink: /2015/05/16/webrtc-peer-connection/
categories:
  - Web 开发
tags:
  - WebRTC
  - HTML5
  - JavaScript
---
## WebRTC 的精髓——点对点连接

[上一篇文章][webrtc-peer-connection]中，主要讲了浏览器怎样获取用户设备上的视频流，并且显示在 HTML5 `<video>` 标签中。这一篇文章则是让这一切变得有用起来：把视频流发送到另一位用户的浏览器上。WebRTC 特有的点对点连接，可以让服务器不必中转大量的视频数据，让通讯的速度、私密性得到更好的保障。这是 WebRTC 相对于 WebSocket 等技术最大的优势，也就是它存在的根本。

## 怎样建立点对点连接

要建立一个点对点连接，并在其上传送视频内容，我们需要两个浏览器互相交换以下信息：

1.视频流的元数据，包括分辨率和编码格式等
2.各自的网络连接情况，包括用于 NAT 穿透的信息

WebRTC 用于实现了以上信息交换，提供给浏览器 JavaScript 平台的 API 就是 `RTCPeerConnection`。

为了完成以上第 1 种信息的交换，我们用 `RTCPeerConnection` 的 `createOffer()` 方法生成一个 Offer，它是以 [SDP][wiki-sdp]（Session Description Protocol，会话描述协议）格式传送的。对方收到 Offer 后，应该生成一个 Answer 并发回，这个 Answer 同样是 SDP 格式的。通信的双方通过调用 `setLocalDescription()` 方法，把自己生成的 SDP 设置成本地描述；通过调用 `setRemoteDescription()` 方法，把对方发给自己的 SDP 设置成远程描述。以上的这个过程，被统称为 [JSEP][wiki-jsep]（JavaScript Session Establishment Protocol，JavaScript 会话建立协议）。

<figure>
  <img src="/media/2015/2015-05-16-webrtc-peer-connection/jsep-architecture.png" alt="jsep-architecture">
  <figcaption>JSEP 结构（via html5rocks.com）</figcaption>
</figure>

对于以上第 2 种信息的交换，则是通过 [ICE][wiki-ice]（Interactive Connectivity Establishment，交互式连接建立）完成的。对于点对点连接最简单的设想是，大家都连接在一个网络中，只要双方都知道对方的 IP 地址，我就可以直接发送数据。但现实永远不会这么简单：如今的网络世界中，绝大部分设备并不是直接连接到互联网上，具有一个公网 IP 地址，而是处在层层的路由器和防火墙的背后，这也就使得直接建立连接变得不可能。不过，如果双方都向一个公网上的服务器发送一个请求，这台服务器可以获取到双方的公网地址，这样就可以让双方知晓怎样和对方进行通讯。这就是 STUN 服务器。

<figure>
  <img src="/media/2015/2015-05-16-webrtc-peer-connection/webrtc-infrastructure.png" alt="webrtc-infrastructure">
  <figcaption>信令交换与 STUN/TURN 服务器（via html5rocks.com）</figcaption>
</figure>

当双方完成了 Offer 和 Answer 的交换后，`RTCPeerConnection` 便利用 STUN 服务器收集 ICE 候选，也就是双方建立连接的多个可能途径，然后在这些候选中挑选最优化的一个，用以建立点对点连接。

STUN 还有一个扩展，即 TURN 服务器。除了实现 STUN 的全部功能外，当双方由于某种原因（如防火墙）还是没法建立点对点连接时，TURN 服务器可以起到中转的作用，让双方可以绕过防火墙进行通讯（事实上绝大多数防火墙被配置为允许从内部向外主动发起的连接）。

## 实例代码

```javascript
// 建立一个 RTCPeerConnection 实例，这里设置了 STUN 或 TURN 服务器
var servers = {
  'iceServers': [
    {
      'url': 'stun:turn.mywebrtc.com'
    },
    {
      'url': 'turn:turn.mywebrtc.com',
      'credential': 'siEFid93lsd1nF129C4o',
      'username': 'webrtcuser'
    }
  ]
};
peerConnection = new RTCPeerConnection(servers);

// 交换 ICE 候选，通过 WebSocket 发送
peerConnection.onicecandidate = function (e) {
  if (e.candidate) {
    console.log(['ICE candidate', e.candidate]);
    socket.emit('message', roomToken, {
      'candidate': e.candidate
    });
  }
};

// 接收到对方添加的视频流时，显示在本地的 <video> 标签中
peerConnection.onaddstream = function (e) {
  remoteMediaStream = e.stream;
  remoteVideo.src = URL.createObjectURL(remoteMediaStream);
};

// 在这里添加上一篇文章中获取到的本地视频流
peerConnection.addStream(localMediaStream);

// 包装一个 Offer
peerConnection.createOffer(gotLocalDescription, handleError);

// 有了 Offer，通过 WebSocket 发送给对方
function gotLocalDescription(desc) {
  peerConnection.setLocalDescription(desc);
  socket.emit('message', roomToken, {
    'sdp': desc
  });
}

// 在 WebSocket 中接收到信息时
socket.on('message', function (message, socketId) {
if (message.sdp) {
    // 接收到 Offer 时，创建 Answer 并发送
    var desc = new RTCSessionDescription(message.sdp);
    peerConnection.setRemoteDescription(desc, function () {
      peerConnection.createAnswer(gotLocalDescription, handleError);
    }, handleError);
  } else {
    // 接收到 ICE 候选时，让 RTCPeerConnection 收集它，稍后它将在这些候选方式中挑选最佳者建立连接
    // 注意：RTCPeerConnection 要在 setLocalDescription 后才能开始收集 ICE 候选
    peerConnection.addIceCandidate(new RTCIceCandidate(message.candidate));
  }
});
```

## 下期预告

本文简述了点对点连接的建立过程中，双方信息交换的流程。

事实上，在 WebRTC 的技术规范中并没有规定这些信息交换要通过什么途径进行，而是把选择的自由留给留给上层的应用程序。在开发 Web App 时，我们可能最先想到的是 WebSocket，但是也可以采用 SIP 或者 Jingle，或者 XMPP 信息服务比如 OpenFire 之类，等等。在本文中，我们使用了 WebSocket 实现信令交换。

信令服务的任务并不止于连接的建立过程。我们还需要把诸如有人加入聊天、有人挂断这样的信息通知给所有客户端。这些信息既可以用与建立连接时相同的机制进行交换，也可以用 WebRTC RTCDataChannel，这是 WebRTC 的数据传送通道，但这个通道只能在点对点连接建立好以后才能使用，也就是说它不能代替 WebSocket 等，但可以在连接建立后把信令交换的任务接管过来。

[WebRTC 之服务器][webrtc-servers] 将会介绍如何使用 node.js 和 socket.io 建立一个 WebSocket 服务器，以提供信令服务；以及如何搭建 STUN/TURN 服务器。


[webrtc-peer-connection]: /2015/04/21/webrtc-video-capture/
[wiki-sdp]: https://en.wikipedia.org/wiki/Session_Description_Protocol
[wiki-jsep]: https://en.wikipedia.org/wiki/JavaScript_Session_Establishment_Protocol
[wiki-ice]: https://en.wikipedia.org/wiki/Interactive_Connectivity_Establishment
[webrtc-servers]: javascript:
