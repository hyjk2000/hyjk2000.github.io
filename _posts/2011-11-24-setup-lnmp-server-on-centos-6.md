---
title: CentOS LNMP 服务器安装配置详解
author: James Shih
layout: post
permalink: /2011/11/24/setup-lnmp-server-on-centos-6/
authorsure_include_css:
  -
categories:
  - Web Server
  - Web 开发
tags:
  - CentOS
  - Linux
  - LNMP
  - MySQL
  - Nginx
  - PHP
  - Web Server
  - Web 服务器
---
LAMP（Linux + Apache + MySQL + PHP）长期以来一直是搭建网站的经济实用之选。但随着 [Igor Sysoev][1] 开发的 [Nginx][2] 服务器渐渐火热起来，LNMP（Linux + Nginx + MySQL + PHP）成为了一个新的选择。

Nginx 服务器比 Apache 服务器更小，运行时消耗资源也少一些，并发性能更高。另外，其配置文件也比 Apache 更简明易懂。不过，目前它的[中文文档][3]还不太完整，也不够新，比起 Apache 的文档来说还有些简陋，所以对于像我这种英文不熟的人来说读起来未免困难。不过，如果熟悉 Apache，上手应该也不会很困难。

在本文中，我将以亲身实践的经历，讲一讲从安装 CentOS 6 开始的 LNMP 服务器配置全过程。

<!--more-->

VPS 用户可以[跳过系统安装和初始配置步骤][4]。

**安装 CentOS 6**

为了节省资源，我选择了 CentOS 6 Minimal 64 位版，刻光盘或[通过 U 盘][5]安装。

<a class="btn" target="_blank" href="http://centos.org/download/">下载 CentOS</a> 或 <a href="http://mirror-status.centos.org/" target="_blank">选择镜像站</a>

具体安装过程不再详述，只需注意安装语言选择英语，时区选择“Asia/Chongqing”或“Asia/Shanghai”，并注意为 root 设置强密码即可。另外，在虚拟机上安装时，最好将网络连接设置为桥接方式（Bridged Adapter）。

**连接网络**

刚安装好的 CentOS 还没有正确连接到网络，我们需要进行设置。一般来说，在服务器上都不使用 DHCP 服务，故以下为例：

<pre>IP:       192.168.1.253
Netmask:  255.255.255.0
Gateway:  192.168.1.1
DNS:      61.139.2.69 218.6.200.139
Hostname: www.mysite.com</pre>

设置 IP 地址等，编辑 /etc/sysconfig/network-scripts/ifcfg-eth0：

<pre>DEVICE="eth0"
HWADDR="08:00:27:E8:5E:86"
NM_CONTROLLED="no"
ONBOOT="yes"
BOOTPROTO=none
IPADDR=192.168.1.253
NETMASK=255.255.255.0
GATEWAY=192.168.1.1</pre>

这里需要注意的有：NM_CONTROLLED 设为 no 以拒绝其他网管软件的管理，ONBOOT 设为 yes 以开机启动，BOOTPROTO 设为 none 以禁用 DHCP。

设置 DNS 服务器，编辑 /etc/resolv.conf：

<pre>nameserver 61.139.2.69
nameserver 218.6.200.139</pre>

设置主机名称，编辑 /etc/sysconfig/network，：

<pre>NETWORKING=yes
HOSTNAME=www.mysite.com</pre>

编辑 /etc/hosts，在此文件下方**添加**一行：

<pre>192.168.1.253    www.mysite.com</pre>

最后，执行命令 service network restart 重启网络连接。

另外，新安装的 CentOS 默认防火墙没有开放 80 端口。请编辑 /etc/sysconfig/iptables，加入以下行，然后执行命令 service iptables restart 重启防火墙：

<pre>-A INPUT -p tcp --dport 80 -j ACCEPT</pre>

**安装必备软件包和设置自动校时**

执行以下命令安装必备软件包：

<pre>yum -y install wget vim sudo screen crontabs ntpdate</pre>

设置每早 6:00 对时，编辑计划任务文件 /etc/crontab，添加一行，：

<pre>0 6 * * * root /usr/sbin/ntpdate time.nist.gov > /dev/null</pre>

**Update(11/06/2012): **在命令的最后加上 > /dev/null 可以隐藏命令输出的信息（但不包括错误信息），这可以让你不会在每次对时后都收到一封邮件，而一旦出现错误时你仍可知晓。如果添加 &#038;> /dev/null 则会隐藏所有输出，包括错误信息。

最后，执行命令 service crond start 启动计划任务。

**配置 SSH**

要进行远程管理，需要配置 SSH。为了提高安全性，我们需要更改 SSH 服务的默认端口，并另行建立新的账户，并禁止 root 远程登录。

如果使用的是 VPS 或独立主机，只能通过 SSH 远程操作时，进行此步时一定要多加小心。更改端口或禁用 root 时，一定要先测试以新的端口和账户能否顺利登录，才能去掉原来的默认端口并禁用 root。这能避免意外情况导致无法再登录服务器。

首先，建立一个新用户，并设置密码：

<pre>useradd -G wheel mysiteadmin
passwd mysiteadmin</pre>

然后，授予 sudo 权限，用 visudo 命令编辑 /etc/sudoers，添加 mysiteadmin 一行：

<pre>## Allow root to run any commands anywhere
root           ALL=(ALL)     ALL
mysiteadmin    ALL=(ALL)     NOPASSWD:ALL</pre>

CentOS 6 Minimal 默认已经安装了 SSH，如果没有安装，请执行 yum install openssh-server。

编辑 SSH 配置文件 /etc/ssh/sshd_config，将原来的“# Port 22”改为以下两行，将 4671 改为你想要的端口号，注意不要与现有软件使用的端口相冲突：

<pre>Port 22
Port 4671</pre>

然后在此文件中搜索“PermitRootLogin yes”，改为“PermitRootLogin no”。

接着在防火墙上开放新的 SSH 端口，编辑 /etc/sysconfig/iptables，加入如下一行，然后执行命令 service iptables restart 重启防火墙：

<pre>-A INPUT -m state --state NEW -m tcp -p tcp --dport 4671 -j ACCEPT</pre>

最后用新的账户和端口登录服务器，测试无问题后，去掉 SSH 配置文件和 iptables 规则中的 22 端口。

至此，系统的安装和初始配置已告完成。可以开始安装 Nginx + MySQL + PHP 了。

<strong id="section_lnmp_installation">安装 LNMP</strong>

首先感谢[军哥（Licess）][6]制作的原版 LNMP 一键安装包。

你可以根据自己的喜好，选择我修改过的定制版 LNMP 一键安装包（lnmp0.7-james-20121206.7z），也可以选择 [LNMP.org][7] 提供的原版安装包。

<a class="btn" href="http://pan.baidu.com/s/1gUkZV" target="_blank">下载 LNMP 一键安装包定制版</a> 或 <a href="http://lnmp.org/download.html" target="_blank">下载 LNMP.org 原版</a>

我修改过的定制版本与原版有以下不同：

1.  更新 Nginx 1.2.4，原版为 0.8
2.  更新 MySQL 5.5.27，原版为 5.1
3.  更新 PHP 5.4.9，原版为 5.2
4.  由于 PHP 5.4 已包含 PHP-FPM 及 PDO-MySQL，故去掉原版中的安装步骤
5.  优化 Nginx 配置文件，便于设置[域名是否使用 www 前缀][8]、URL Rewrite、上传目录安全防护等

安装过程比较漫长，故安装前请用 screen -S lnmp 创建新会话，一旦出现网络不稳定导致 SSH 断线等情况，只需用 screen -r lnmp 命令返回会话即可，不会造成安装过程中断。安装完成后，用 exit 命令退出会话。如果系统没有安装 screen，请执行 yum install screen 命令。

将下载到的 LNMP 安装包通过 [WinSCP][9]、FTP 等直接上传到服务器的 /home 目录下。如果使用原版安装包，则可以直接通过 wget 命令直接下载到服务器上。下载好后，便可以开始解压安装了：

<pre>7z e lnmp0.7-james-20121206.7z
cd lnmp0.7-james
./sh centos.sh | tee lnmp.log （Debian、Ubuntu 系统则相应选择 debian.sh 和 ubuntu.sh）</pre>

然后，根据提示输入域名和 MySQL root 密码，提示“Press any key to start&#8230;”后按任意键开始安装。

根据服务器配置及网络连接速度不同，安装过程可能会持续 30-60 分钟不等。

安装完成后，可以试试能否访问。用 /root/lnmp {start|stop|reload|restart|kill|status} 即可控制 LNMP 的运行。

要安装 ionCube、eAccelerator、Pure-FTPd、VSFTPd 等，可以参见 LNMP.org 的[安装说明][10]。

**延伸阅读：**  
详细了解 Linux 的网络连接设置：[鸟哥的 Linux 私房菜 &#8211; 连上 Internet][11]  
详细了解 Linux 计划任务：[鸟哥的 Linux 私房菜 &#8211; 例行性工作排程 (crontab)][12]  
设置更安全的 SSH 登录：[设置 SSH 通过密钥登录][13]  
了解 PHP 上传相关设置：[PHP 上传文件故障排除][14]  
了解为 LNMP 配置添加版本控制：[用 Subversion 管理网站程序][15]

 [1]: http://sysoev.ru/en/
 [2]: http://nginx.org
 [3]: http://wiki.nginx.org/Chs
 [4]: #section_lnmp_installation
 [5]: http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/ "Universal USB Installer"
 [6]: http://blog.licess.org/
 [7]: http://lnmp.org
 [8]: /2011/11/24/enforce-with-www-or-without-www/ "为何要进行此项设置？"
 [9]: http://winscp.net/eng/docs/lang:chs
 [10]: http://lnmp.org/install.html
 [11]: http://linux.vbird.org/linux_server/0130internet_connect.php
 [12]: http://linux.vbird.org/linux_basic/0430cron.php
 [13]: /2012/03/16/how-to-set-up-ssh-keys/
 [14]: /2011/10/19/php-upload-troubleshooting/
 [15]: /2012/04/18/setup-subversion-on-web-server/
