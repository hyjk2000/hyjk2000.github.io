---
title: 不准蹭网！No Piggybacking!
author: James Shih
layout: post
permalink: /2010/03/06/%e4%b8%8d%e5%87%86%e8%b9%ad%e7%bd%91%ef%bc%81no-piggybacking/
spaces_666edbae79c1d30694f5e5d213cfdf0f_permalink:
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!962')?authkey=72j5ZQnBJYQ%24"
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!962')?authkey=72j5ZQnBJYQ%24"
categories:
---
古有“盗打电话”，今有“蹭上无线网”。WLAN的特点就决定了，信号是挡不住的。自从无线路由器白菜价以后，走到哪里一搜都是一堆无线网，而且大部分都是没加密的，随便上。如果说就看看网页，让你蹭了也就蹭了。但是有很多人抱怨自己的网速怎么一下子就慢了，有个人在离你不远的地方用你的网开BT呢，那带宽占得只给你留几Kbps，你还能干什么呢？发摩斯电码吗？

后来终于有越来越多的人发现自己在开网吧，于是用上了<a href="http://zh.wikipedia.org/zh-cn/WEP" target="_blank">WEP</a>（Wired Equivalent Privacy）加密方法。好吧，这个名字听起来不错，“有线等效加密”，听起来就跟有线网一样安全（至少在它诞生的1999年是的）。而如今，它已经很容易被破解，以至于有无良商贩用劣质山寨高功率无线网卡搭配个BT3光盘，做成“蹭网卡”来卖，至今畅销不衰。即使是完全不懂网络技术的普通人，用BT3也可以很容易地破解WEP加密，即使把密码设得很复杂，破解成功也只是个时间问题。况且有不少人WEP密码、QQ密码、网游密码、E-mail密码、银行卡密码、网银密码……全都一样，那破解者成功后的收获可真不小。

<strong>现在你了解了吧，这种“破网卡”其实就是一个很破的无线网卡加上一个网上到处都能下载到的软件。它只能破解WEP加密的WLAN，而且你还得冒着被山寨无线网卡强劲的高功率无线电信号辐射成蜘蛛侠、金刚狼或者绿巨人的风险。</strong>

这么说吧，一般正规的无线网卡的发射功率通常是10mW左右，而这种山寨货一般都标称1W，相差100倍。

要防止被蹭网，办法很简单，用<a href="http://zh.wikipedia.org/zh-cn/WPA" target="_blank">WPA</a>（Wi-Fi Protected Access）或802.11i的<a href="http://zh.wikipedia.org/zh-cn/WPA2" target="_blank">WPA2</a>加密方法就行了。任你“卡王卡皇”，全部没招儿。

<strong>措施一：启用WPA或WPA2加密方法，推荐WPA2-PSK[AES]加密方法</strong>

我们要进入无线路由器的配置界面进行设置。在浏览器中输入“192.168.1.1”，这个地址各个品牌的路由器并不相同。一般TP-Link和NETGEAR是192.168.1.1，D-Link是192.168.0.1。默认的用户名和密码一般都是“admin”，但NETGEAR的密码是“password”。具体可以查看路由器的说明书。

进入你的路由器的无线安全设置，以下是NETGEAR路由器上的无线安全设置方法，其他品牌类似。如果你在输入密码之后仍然无法上网，那就选择“WPA-PSK[TKIP] + WPA2-PSK[AES]”。某些品牌，如TP-Link，没有此选项而是让你选择WPA还是WPA2，那就直接选择“自动”。

注意我把“频道”（信道）设置为自动，这是推荐设置。如果你的无线路由器没有自动这个选项，那么你就看看周围的WLAN的信道都是多少，你设置的时候尽量跟他们不要重叠，而且距离尽量远。如果你实在不知道咋设，就设为6。很多无线网不稳定的原因都是信道设置不当导致的。

![NETGEAR_WirelessSecurity](/media/legacy/2010/03/NETGEAR_WirelessSecurity.png)

<strong>措施二：【非必需的、辅助性的加强措施】禁用SSID广播</strong>

SSID（Service Set Identifier）也就是你网络的名称。禁用SSID广播可以让别人不容易发现你的WLAN，从而降低被攻击的可能性。但如果你有新设备要加入网络，你需要手动输入你的SSID。

设置方法是，在路由器管理界面中找到并禁用“SSID广播”的选项，请参考路由器说明书。

<strong>措施三：【非必需的、辅助性的加强措施】设置MAC地址允许列表</strong>

每块无线网卡都有自己的一个MAC地址，如果没有被修改的话，这个地址是唯一的。可以在无线路由器里设置一个MAC地址列表，只有列表中有的MAC地址才可以访问WLAN，这样就提高了WLAN的安全性。这个方法的缺点是，如果你有新设备要加入网络，必需现在路由器管理界面中添加它的MAC地址，它才能够访问WLAN。

获得自己的网卡MAC地址：在命令提示符中输入“ipconfig /all”，找到无线网络连接，其下面的Physical Address就是MAC地址。

添加到允许列表：在不同品牌的无线路由器中，这个功能的称呼可能有“无线网卡访问列表”、“MAC地址过滤”等等，请参考路由器说明书。以下是NETGEAR路由器上的设置界面，其他品牌类似。

![NETGEAR_MAC](/media/legacy/2010/03/NETGEAR_MAC.png)
