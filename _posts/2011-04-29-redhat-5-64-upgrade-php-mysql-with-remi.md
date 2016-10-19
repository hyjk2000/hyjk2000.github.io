---
title: Redhat 5.4 64位版 通过 remi 升级 PHP 和 MySQL
author: James Shih
layout: post
permalink: /2011/04/29/redhat-5-64-upgrade-php-mysql-with-remi/
jabber_published:
  - 1304045604
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"13298777";s:7:"blog_id";s:8:"12911913";s:9:"mod_stamp";s:19:"2011-04-29 05:32:19";}'
categories:
  - Linux
tags:
  - MySQL
  - PHP
  - Redhat
  - Remi
---
**注意**：如果是 Redhat 6.0，应该相应地安装 epel-release-6 和 remi-release-6。另外，在 SSH 里敲命令之前，先到 remi 和 epel 的下载地址去看看现在最新的版本号。

首先，安装 remi 和 epel：

<blockquote style="font:12px consolas,monotype;">
  <p>
    wget http://download.fedora.redhat.com/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm
  </p>
  
  <p>
    wget http://rpms.famillecollet.com/enterprise/5/remi/x86_64/remi-release-5-8.el5.remi.noarch.rpm
  </p>
  
  <p>
    rpm -Uvh remi-release-5*.rpm epel-release-5*.rpm
  </p>
</blockquote>

然后编辑3个文件，在它们的最后加上一行“priority=1”：

<blockquote style="font:12px consolas,monotype;">
  <p>
    vi /etc/yum.repos.d/remi.repo<br />vi /etc/yum.repos.d/epel.repo<br />vi /etc/yum.repos.d/epel-testing.repo
  </p>
</blockquote>

最后敲命令升级 PHP 和 MySQL：

<blockquote style="font:12px consolas,monotype;">
  <p>
    yum &#8211;enablerepo=remi update php* mysql*
  </p>
</blockquote>

PHP 和 MySQL 会自动升级到当前的最新稳定版本。

**Update(11/17/2011): **  
对于 RHEL 6.0 x86_64，现在的 EPEL 和 REMI 的下载地址为：  
<http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-5.noarch.rpm>  
<http://rpms.famillecollet.com/enterprise/remi-release-6.rpm>