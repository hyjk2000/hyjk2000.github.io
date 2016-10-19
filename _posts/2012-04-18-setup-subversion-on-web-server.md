---
title: 用 Subversion 管理网站程序
author: James Shih
layout: post
permalink: /2012/04/18/setup-subversion-on-web-server/
categories:
  - Web Server
  - Web 开发
tags:
  - Subversion
  - SVN
  - Version Control
  - Web Server
  - 版本控制
  - 网站服务器
---
在我去年的文章 [CentOS LNMP 服务器安装配置详解][1]中，讲解了如何利用 CentOS 6 和 LNMP 一键安装包快速搭建一个 Web 服务器。但是用这个流程建好的服务器，默认还没有远程更新代码的服务，比如 FTP 等等。不过，就更新网站程序而言，FTP 不是个好选择，尤其是有多位人员在更新服务器上的程序代码时。在这种情况下，[Subversion (SVN)][2] 是更好的选择。

在网站服务器上搭建好 Subversion 后，就可以通过 SVN 客户端来部署和管理程序代码了。每次对线上的程序代码进行修改时，都会产生一个新的版本号，并且随时可以回到以前的版本或下载以前版本的程序代码。此外，当多个人同时修改后，SVN 能够自动合并修改，并提示可能的冲突让你手动解决。

下面来介绍服务器上的安装步骤：

<!--more-->

**1. 安装 Subversion**

在 CentOS 上安装 Subversion 很容易，只要一条命令即可：

```bash
yum install subversion
```

在其他 Linux 发行版上，可以使用相应的包管理器来安装。例如 Ubuntu 上：

```bash
apt-get install subversion
```

**2. 建立 SVN 仓库**

LNMP 一键安装包安装完成后，网站文件默认存放在 /home/wwwroot 目录。我们可以在 /home 目录下新建一个文件夹 repo，用来放置 SVN 仓库。

```bash
mkdir -p /home/repo/wwwroot
svnadmin create /home/repo/wwwroot
```

SVN 仓库建立完成后，在目录下会生成一些配置文件。我们接着修改这些配置文件。

**3. 设置 SVN 账户及权限**

首先修改 SVN 仓库的配置文件 /home/repo/wwwroot/conf/svnserve.conf 文件，修改以下项目：

```
[general]
anon-access = none
auth-access = write
password-db = passwd
authz-db = authz
realm = wwwroot
```

然后建立账户，修改 /home/repo/wwwroot/conf/passwd 文件，添加账户和密码：

```
[general]
[users]
user1 = password1
user2 = password2
user3 = password3
user4 = password4
user5 = password5
```

接下来是基于路径的访问控制，修改 /home/repo/wwwroot/conf/authz 文件，指定账户的权限：

```
[groups]
group1 = user1,user2

[/]              # 根目录权限设置
user3 = rw       # user3权限是：读写
user4 = r        # user4权限是：只读
user5 =          # user5没有任何权限
@group1 = rw     # group1组权限是：读写
```

**4. 设置自动更新网站程序目录**

我们希望在通过 SVN 提交代码后，自动更新网站程序目录，方法是设置“钩子”。“钩子”是一段 Shell 脚本，在 SVN 发生某个事件后自动被执行一次。通过设置 post-commit 钩子，可以让 SVN 在接到我们新提交的代码后，自动更新网站程序目录中的文件。

添加 /home/repo/wwwroot/hooks/post-commit 文件：

```bash
#!/bin/sh
REPOS="$1"
REV="$2"
svn update /home/wwwroot --username user1 --password password1
```

别忘了设置为可执行：

```bash
chmod u+x /home/repo/wwwroot/hooks/post-commit
```

**5. 启动 svnserve**

SVN 可以通过 Apache 等 Web 服务器来提供服务，也可以独立提供服务，两者各有优缺点，这里我们选择独立运行。启动 svnserve 后，就可以通过网络远程访问到 SVN 仓库了。

```bash
svnserve -d -r /home/repo/wwwroot    # 启动
killall svnserve                     # 停止
```

也可以设置 svnserve 随系统启动，添加 /etc/init.d/svnserve 文件：

```bash
#!/bin/bash
#chkconfig: 2345 70 40  
#description: Starts the Subversion serve in daemon mode

### BEGIN INIT INFO
# Provides:          svnserve
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the svnserve
# Description:       starts the subversion serve in daemon mode
### END INIT INFO

svnserve -d -r /home/repo/wwwroot
```

然后添加到系统启动项中：

```bash
chmod +x svnserve
chkconfig --add svnserve
```

另外，别忘了在防火墙上开放 svnserve 使用的端口，默认是 3690 端口。

**6. 签出到网站目录**

SVN 仓库建立完成后，先初始签出到网站目录，以后就可以通过 svn update 来自动使网站目录中的文件和 SVN 仓库中的文件同步了。

```bash
svn checkout file:///home/repo/wwwroot /home/wwwroot --username user1 --password password1
```

**7. 拒绝 Web 访问 .svn 目录**

签出到网站目录后，网站目录下会生成一些 .svn 文件夹。这些文件夹中存放的是 SVN 系统使用的文件，我们不希望用户通过 Web 浏览到这些文件。通过修改服务器配置文件，我们可以做到：

*Nginx：*  
在 nginx.conf 的 server 块中添加：

```
location ~ /\. { access_log off; log_not_found off; deny all; }
```

*Apache：*  
在 httpd.conf 中添加：

```
```
<FilesMatch "\.svn/.*"```
>
   Order Allow,Deny
   Deny from all
```
</FilesMatch```
>
```

重新载入服务器配置文件，让设置生效即可。

**8. 通过 SVN 客户端提交程序文件**

使用刚才添加的账户和密码连接到 svn://【服务器地址】/ 就可以访问 SVN 仓库了。

Windows 上最流行的 SVN 客户端是 [TurtoiseSVN][3]。而对于 Mac 和 Linux，则通常使用 SVN 命令行客户端。关于这些客户端的使用方法，Sina App Engine 的文档中心有一个[很不错的入门教程][4]可以参考。

至此，我们完成了 SVN 服务的搭建。现在你可以通过 SVN 来更好地管理网站服务器上的程序文件了。

 [1]: /2011/11/24/setup-lnmp-server-on-centos-6/
 [2]: http://subversion.apache.org/
 [3]: http://tortoisesvn.net/
 [4]: http://sae.sina.com.cn/?m=devcenter&#038;catId=212
