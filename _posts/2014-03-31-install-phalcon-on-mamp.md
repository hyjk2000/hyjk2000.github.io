---
title: 为 MAMP 安装 Phalcon 框架
quote: Phalcon 是一个用 C 语言编写的，号称是速度最快、占用资源最少的 PHP 框架。
author: James Shih
layout: post
permalink: /2014/03/31/install-phalcon-on-mamp/
authorsure_include_css:
  - 
categories:
  - PHP
  - Web 开发
tags:
  - Mac
  - MAMP
  - OS X
  - Phalcon
  - PHP
---
[Phalcon][1] 是一个用 C 语言编写的，号称是速度最快、占用资源最少的 PHP 框架。它以一个 PHP 扩展的形式安装，与 CodeIgniter、CakePHP 等框架有显著的不同。

Phalcon 在 Windows 上的安装很简单，只要在官方网站上找到对应 PHP 版本的 DLL，放进 PHP 目录，然后在 `php.ini` 里加上就行了。但在 Linux 和 Mac 上需要自己编译。

在 Mac 上做 PHP 开发，很多人都用 [MAMP][2]。情况比较麻烦，因为除了 MAMP 以外，OS X 还自带了一个 PHP；而且 MAMP 没有自带 PHP 的源码。所以需要一些额外的步骤。

<!--more-->

## 准备编译环境

首先，你得有一个包管理器，比如 [Homebrew][3]，用来安装一些工具。另外，还要安装 [Xcode][4] 或者只安装它的[命令行工具][5]，才能进行编译。

接下来，用 Homebrew安装一些工具：

    $ brew install autoconf automake libtool

## 修改环境变量

现在，如果你在终端使用 PHP，实际上用的是 OS X 自带的那个：

    $ which php
    /usr/bin/php

修改环境变量，让终端调用 MAMP 里的 PHP：

    $ export PATH=/Applications/MAMP/bin/php/php5.4.4/bin:$PATH

要让设置在下次启动终端时保留，把以上这句加入 `~/.bash_profile` 文件里。

检查设置是否生效：

    $ which php
    /Applications/MAMP/bin/php/php5.4.4/bin/php

## 下载 PHP 源码

用 `php --version` 获得 PHP 的版本，然后在 [php.net][6] 下载对应的源码包。

    $ php --version
    PHP 5.4.4 (cli) (built: Jul  4 2012 17:28:56) 
    Copyright (c) 1997-2012 The PHP Group
    Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
    
    $ curl http://museum.php.net/php5/php-5.4.4.tar.bz2 | tar -xj
    $ mkdir /Applications/MAMP/bin/php/php5.4.4/include
    $ mv php-5.4.4 /Applications/MAMP/bin/php/php5.4.4/include/php
    $ cd /Applications/MAMP/bin/php/php5.4.4/include/php
    $ ./configure

## 安装 Phalcon

    $ curl -L -o cphalcon-master.zip https://github.com/phalcon/cphalcon/archive/master.zip
    $ unzip cphalcon-master.zip
    $ cd cphalcon-master/build
    $ sudo ./install

## 修改 php.ini

首先确定 `php.ini` 的位置：

    $ php --ini
    Configuration File (php.ini) Path: /Applications/MAMP/bin/php/php5.4.4/conf
    Loaded Configuration File:         /Applications/MAMP/bin/php/php5.4.4/conf/php.ini
    Scan for additional .ini files in: (none)
    Additional .ini files parsed:      (none)

然后在 php.ini 中加上一条：

    extension=phalcon.so

## 检查安装

除了可以在 `phpinfo()` 输出的页面中查找 Phalcon，也可以这样检查：

    $ php -r 'phpinfo();' | grep Phalcon
    Phalcon Framework => enabled
    Phalcon Version => 1.3.3

 [1]: http://phalconphp.com/
 [2]: http://www.mamp.info/
 [3]: http://brew.sh/
 [4]: https://developer.apple.com/xcode/
 [5]: https://developer.apple.com/downloads/index.action
 [6]: http://php.net/