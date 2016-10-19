---
title: HTML：textarea标签的wrap属性
author: James Shih
layout: post
permalink: /2010/04/07/html%ef%bc%9atextarea%e6%a0%87%e7%ad%be%e7%9a%84wrap%e5%b1%9e%e6%80%a7/
spaces_666edbae79c1d30694f5e5d213cfdf0f_permalink:
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!1293')?authkey=72j5ZQnBJYQ%24"
  - "http://cid-6ed4d3e2ba61f4c5.users.api.live.net/Users(7986241009977783493)/Blogs('6ED4D3E2BA61F4C5!102')/Entries('6ED4D3E2BA61F4C5!1293')?authkey=72j5ZQnBJYQ%24"
categories:
  - HTML/CSS
---
<div id="msgcns!6ED4D3E2BA61F4C5!1293" class="bvMsg">
  <p>
    如果你希望用户在<textarea>里输入内容时自动换行，那么需要设置<strong>wrap=”virtual”</strong>或<strong>wrap=”physical”</strong>。这两个值的差别主要是实际提交到服务器的内容有区别，可以举个例子来说明。
  </p>
  
  <p>
    假设下面这个情况：用户输入了一行长度大于文本区宽度的文字，然后按回车，又输入了一行文字。
  </p>
  
  <p>
    <a href="/media/legacy/2010/04/textarea_wrap_201004075b25d.png" rel="WLPP"><img style="border-bottom:0;border-left:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;" title="textarea_wrap_20100407" border="0" alt="textarea_wrap_20100407" src="/media/legacy/2010/04/textarea_wrap_201004075b25d.png?w=300" width="550" height="285" /></a>
  </p>
  
  <p>
    <strong>wrap=”virtual”</strong>：可以实现文本区内的自动换行，以改善对用户的显示。但提交到服务器的内容中，文本只有在用户按回车键的地方才换行。
  </p>
  
  <p>
    <strong>wrap=”physical”</strong>：可以实现文本区内的自动换行，并且按原样提交到服务器。当需要用户能够对提交的内容做到“所见即所得”时，这个设置非常有用。
  </p>
  
  <p>
    当设置为wrap=”off”时，将使用浏览器的默认设置。不同的浏览器的默认设置是不一样的，有些等同于wrap=”wrap”，即不自动换行，有些则等同于wrap=”virtual”。
  </p>
</div>