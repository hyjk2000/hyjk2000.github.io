---
title: 修复 IE 主页篡改
author: James Shih
layout: post
permalink: /2011/12/15/restore-hijacked-ie-homepage/
categories:
tags:
  - homepage hijacking
  - IE
  - registry
  - 主页篡改
  - 注册表
---
最近公司有一些同事的电脑中了恶意软件，IE 主页被篡改成一些垃圾网址导航网站，并且无法通过修改 Internet 选项中的主页选项恢复。在鄙视这些垃圾站的同时，也深深认识到在公司内推广使用 Google Chrome 浏览器的必要性。本文将介绍应该如何通过修改注册表修复被篡改的 IE 主页。

<!--more-->

**首先扫描病毒**

正在运行的恶意软件会不断修改注册表，使你的修改前功尽弃。所以首先你需要更新杀毒软件，并执行快速扫描。

**检查 IE 快捷方式**

系统新安装时，桌面上的 IE 图标实际上不是一般意义上的快捷方式，因为打开它的属性时，弹出的是 Internet 选项，而不是快捷方式属性。所以当你发现 IE 图标上有小箭头时，可能恶意软件删除了你的 IE 图标，又新建了一个带有垃圾网站的快捷方式。这样你每次点击打开 IE 时当然就会直接打开垃圾网站了。

选中 IE 快捷方式，按 Alt+Enter 键打开属性。在“目标”一栏中，清除末尾的垃圾网站网址。

**修改注册表**

如果上述操作没能解决问题，则需要修改注册表。新建文本文件 fix-ie-home.reg，填入以下内容保存，双击文件导入即可：

	Windows Registry Editor Version 5.00

	[HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\Main]
	"Start Page"="about:blank"
	"Search Page"="http://go.microsoft.com/fwlink/?LinkId=54896"

	[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\MAIN]
	"Security Risk Page"="about:SecurityRisk"
	"Extensions Off Page"="about:NoAdd-ons"
	"Default_Search_URL"="http://go.microsoft.com/fwlink/?LinkId=54896"
	"Default_Page_URL"="http://go.microsoft.com/fwlink/?LinkId=69157"
	"Start Page"="http://go.microsoft.com/fwlink/?LinkId=69157"
	"Local Page"="C:\\Windows\\System32\\blank.htm"
	"Search Page"="http://go.microsoft.com/fwlink/?LinkId=54896"

	[HKEY_CLASSES_ROOT\Applications\iexplore.exe\shell\open\command]
	@="\"C:\\Program Files\\Internet Explorer\\iexplore.exe\" %1"

	[HKEY_CLASSES_ROOT\CLSID\{871C5380-42A0-1069-A2EA-08002B30309D}\shell\OpenHomePage\Command]
	@="\"C:\\Program Files\\Internet Explorer\\iexplore.exe\""

	[HKEY_CURRENT_USER\Software\Policies\Microsoft\Internet Explorer\Control Panel]
	"HomePage"=-

	[HKEY_CURRENT_USER\Software\Policies\Microsoft\Internet Explorer\Main]
	"Start Page"=-

别忘了文件末尾留一行空行。