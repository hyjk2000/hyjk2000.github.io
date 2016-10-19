---
layout: post
title: "本地网络禁止 22 端口出站时怎样使用 Git+SSH"
quote: "配置 SSH 通过其他端口或代理服务器连接指定的域名"
date: "2016-02-24 13:12:27 +0800"
slug: git-ssh-with-port-22-outbound-blocked
author: James Shih
categories:
  - Linux
tags:
  - Git
  - SSH
---
如果你习惯用 SSH 协议而不是 HTTPS 去连接 GitHub，那么你很可能遇到过：当你 push 代码到 GitHub 的时候出现 `port 22: Connection refused` 的错误。大部分情况下，这是因为你所在的本地网络防火墙禁止 22 端口出站连接。事实上，对于企业网络安全而言，这么做是[非常有必要的](https://serverfault.com/a/25566)。但是，突然你就不能 push 代码了，我现在急着提交一个 hotfix，怎么办？

## 使用后备 Git+SSH 端口

幸运的是，常用的 Git 托管服务大多提供了通过 HTTPS 的 443 端口连接 Git+SSH 服务的功能。只需要修改你的 SSH 配置文件即可启用。

编辑 `~/.ssh/config` 文件：

```
Host github.com
  Hostname ssh.github.com
  Port 443

Host bitbucket.org
  Hostname altssh.bitbucket.org
  Port 443

Host gitlab.com
  Hostname altssh.gitlab.com
  Port 443
```

## 使用代理服务器

如果你使用的 Git 托管服务没有提供后备 Git+SSH 端口，或者你所在的本地网络必须通过代理服务器访问外网，则可以利用 [nc](http://linux.die.net/man/1/nc) 来达成。例如：

```
Host git.somecompany.com
  ProxyCommand nc -x 10.0.146.35:3128 %h %p
```
