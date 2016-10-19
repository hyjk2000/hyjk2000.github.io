---
title: 用X-UA-Compatible设置IE8兼容模式
author: James Shih
layout: post
permalink: /2010/04/22/%e7%94%a8x-ua-compatible%e8%ae%be%e7%bd%aeie8%e5%85%bc%e5%ae%b9%e6%a8%a1%e5%bc%8f/
spaces_666edbae79c1d30694f5e5d213cfdf0f_permalink:
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!1297')?authkey=72j5ZQnBJYQ%24"
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!1297')?authkey=72j5ZQnBJYQ%24"
categories:
  - HTML/CSS
---
<div id="msgcns!6ED4D3E2BA61F4C5!1297" class="bvMsg">
  <p>
    IE8终于不完美地支持了一些互联网标准，但IE7和以前的版本遗留下来的历史问题确实很让人头疼。相信很多人被IE7以前版本怪异的行为搞得焦头烂额。如果你遇到了兼容性上的问题，不妨参考参考这篇文章。
  </p>
  
  <p>
    有时候，IE8会自作聪明地使用兼容模式来显示你的网站（也可能是你的主机自作聪明的设置），本来符合标准的网页被显示得七零八落。这时你就希望可以将IE设置为不使用兼容模式。另一种情况，一些学校和政府的老网站，为老的IE版本所优化，在标准浏览器上显示出现错乱。这时你可能希望IE8自动打开兼容模式。
  </p>
  
  <p>
    <strong>方法1：在页面中添加meta标签</strong>
  </p>
  
  <p>
    在页面顶部的HEAD部分中添加一个meta标签：
  </p>
  
  <blockquote>
    <p>
      <font face="Consolas"><meta http-equiv="X-UA-Compatible" content="<font color="#ff0000">IE=edge</font>" /></font>
    </p>
  </blockquote>
  
  <p>
    红色部分可以替换为以下值来实现不同设置：
  </p>
  
  <blockquote>
    <p>
      <font color="#ff0000">IE=EmulateIE7</font>：让IE8以兼容模式显示页面，模仿IE7的行为
    </p>
    
    <p>
      <font color="#ff0000">IE=5</font>、<font color="#ff0000">IE=7</font> 或 <font color="#ff0000">IE=8</font>：选择其中一种兼容性模式
    </p>
    
    <p>
      <font color="#ff0000">IE=edge</font>：让IE8使用最高级别的可用模式（不建议在生产环境中使用）
    </p>
  </blockquote>
  
  <p>
    <strong>方法2：设置Web服务器</strong>
  </p>
  
  <p>
    请参见MSDN文档：
  </p>
  
  <ul>
    <li>
      <a href="http://msdn.microsoft.com/zh-cn/library/cc817572.aspx">在 IIS 上实现 META 切换</a> <li>
        <a href="http://msdn.microsoft.com/zh-cn/library/cc817573.aspx">在 Apache 上实现 META 切换</a>
      </li></ul> 
      <p>
         
      </p>
      
      <p>
        此外，通过JavaScript可以取得当前的兼容性模式：
      </p>
      
      <blockquote>
        <p>
          <font face="Consolas">javascript:alert(document.documentMode);</font>
        </p>
      </blockquote>
      
      <p>
        如果使用了IE8模式，document.documentMode会返回"8"。
      </p></div>