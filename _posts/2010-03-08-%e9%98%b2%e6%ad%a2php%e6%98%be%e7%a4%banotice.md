---
title: 防止PHP显示Notice
author: James Shih
layout: post
permalink: /2010/03/08/%e9%98%b2%e6%ad%a2php%e6%98%be%e7%a4%banotice/
spaces_666edbae79c1d30694f5e5d213cfdf0f_permalink:
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!963')?authkey=72j5ZQnBJYQ%24"
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!963')?authkey=72j5ZQnBJYQ%24"
tagazine-media:
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"13298777";s:7:"blog_id";s:8:"12911913";s:9:"mod_stamp";s:19:"2011-03-23 15:17:03";}'
  - 'a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"13298777";s:7:"blog_id";s:8:"12911913";s:9:"mod_stamp";s:19:"2011-03-23 15:17:03";}'
categories:
  - PHP
  - Web 开发
---
<div id="msgcns!6ED4D3E2BA61F4C5!963" class="bvMsg">
  <p>
    在把自己辛辛苦苦编写的PHP程序部署到服务器上的时候，相信这样的提示很多人都见过：
  </p>
  
  <blockquote>
    <p>
      <span style="font-family:'Courier New';">PHP Notice:  Undefined variable<br /> PHP Notice:  Undefined index</span>
    </p>
  </blockquote>
  
  <p>
    看看吧，你有没定义的变量直接使用了。不过编PHP的时候本来就不像C++那么严格，编程的时候经常还会利用这个特点。PHP的默认设置是显示这些提示，这会造成页面无法正常显示出来。
  </p>
  
  <p>
    如果你要说“PHP，我知道我没定义，就这么地了，你甭多嘴了成不？”，那也容易。只要打开服务器的php.ini文件，找到以下选项：
  </p>
  
  <blockquote>
    <p>
      <span style="font-family:'Courier New';font-size:x-small;">;   &#8211; Show all errors, except for notices and coding standards warnings<br /> ;<br /> ;error_reporting = E_ALL & ~E_NOTICE<br /> ;<br /> ;   &#8211; Show all errors, except for notices<br /> ;<br /> ;error_reporting = E_ALL & ~E_NOTICE | E_STRICT<br /> ;<br /> ;   &#8211; Show only errors<br /> ;<br /> ;error_reporting = E_COMPILE_ERROR|E_RECOVERABLE_ERROR|E_ERROR|E_CORE_ERROR<br /> ;<br /> ;   &#8211; Show all errors, except coding standards warnings<br /> ;<br /> error_reporting = E_ALL</span>
    </p>
  </blockquote>
  
  <p>
    把“error_reporting = E_ALL & ~E_NOTICE”前面的分号去掉，“error_reporting = E_ALL”前面加一个分号，保存后重启Web服务器软件，就行了！
  </p>
  
  <p>
    <strong>Update(03/09/2010):</strong> 须知并不是每个服务器你都有权去调整这些参数，所以最好在编程的时候规范些，以利程序的发布。
  </p>
  
  <p>
    <strong>Update(03/23/2011):</strong> PHP 函数 error_reporting 可以设置错误报告的级别。如 error_reporting(0) 将关闭所有错误提示，error_reporting(E_ALL ^ E_NOTICE) 则显示除 Notice 以外的错误信息（PHP 默认值）。详情请参见 <a href="http://http//docs.php.net/manual/zh/function.error-reporting.php" target="_blank">PHP Manual: error_reporting</a>。
  </p>
</div>