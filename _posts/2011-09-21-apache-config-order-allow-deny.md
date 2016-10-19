---
title: Apache 配置文件中的 Order Allow Deny
author: James Shih
layout: post
permalink: /2011/09/21/apache-config-order-allow-deny/
categories:
  - Web Server
  - Web 开发
tags:
  - allow
  - Apache
  - deny
  - httpd
  - httpd.conf
  - order
  - 服务器配置
---
对于新手来说，Apache 配置文件的可读性确实差了点，在不看文档的情况下很多都不知所云。一个典型的例子就是 Order、Allow、Deny。

如果配置文件中这样写，你会认为是什么意思？

```
Order Allow,Deny
Allow from 1.2.3.4
Deny from all
```

<!--more-->

不注意的话，很可能会被看成“除 IP 为 1.2.3.4 的用户外，拒绝其他用户访问”。OK，恭喜你答错了。

因为 **Order Allow,Deny** 的含义为**先处理 Allow，再处理 Deny**。这样一来，Deny 项的优先级被提高，并且未匹配 Allow 的情况会默认以 Deny 处理。最后，Deny from all 覆盖了 Allow from 1.2.3.4，造成所有用户被拒绝访问。

要更正这个问题，只要去掉最后一行就行了：

```
Order Allow,Deny
Allow from 1.2.3.4
```

先处理 Allow from 1.2.3.4，然后，没有匹配的用户统统 Deny。

一般来说，如果你需要一个**白名单**，就用 **Order Allow,Deny**；如果是**黑名单**，就用 **Order Deny,Allow**。
