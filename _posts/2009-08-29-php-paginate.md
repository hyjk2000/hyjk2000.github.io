---
title: 在PHP应用程序中用Gmail的样式实现分页显示数据库内容
author: James Shih
layout: post
permalink: /2009/08/29/php-paginate/
categories:
  - PHP
  - Web 开发
---
<div id="msgcns!6ED4D3E2BA61F4C5!544" class="bvMsg">
  <p>
    当数据库表中的记录条数增多的时候（例如有10000条），你似乎不太可能在一页中把它们全都显示出来。否则SQL Query执行时间和数据量会变得很大，造成页面载入缓慢。而且客户端和服务器端的内存都会被很快吞噬掉，总之会造成糟糕的用户体验。
  </p>

  <p>
    这时候我们需要一个方便直观的翻页机制。我们很容易注意到Gmail的邮件列表翻页机制：
  </p>

  <p>
    <a href="/media/legacy/2009/08/clip_image0015b45d.gif" rel="WLPP"><img style="display:inline;border-width:0;" title="clip_image001" border="0" alt="clip_image001" src="/media/legacy/2009/08/clip_image0015b45d.gif?w=300" width="548" height="367" /></a>
  </p>

  <p>
    邮件列表的顶部提供了一些链接，例如刷新、更旧（下翻一页）、最旧（最后页）、全选等等，还有一些按钮可以针对选定的邮件进行操作，还有一个指示器显示出当前显示的是哪些邮件。按动“更旧”来翻到下一页，你会发现下一页的数据通过AJAX传送回来，并不必刷新整个页面，而且速度很快。我们认为这个翻页界面具有良好的用户体验，所以决定仿照它来制作一个自己的翻页控件。
  </p>

  <p>
    不像ASP.NET可采用专门控件通过ADO.NET绑定数据源来实现分页显示和AJAX更新，PHP程序员需要手动实现这一功能，听上去似乎很难，但可以获得更多自主性和灵活性，而且并不麻烦。使用PHP的数据库插件可以非常方便直观地访问数据库，在这一点上我认为PHP比ADO.NET罗哩罗嗦的绑定代码更为有趣。
  </p>

  <p>
    OK，第一步我们先来设计客户端。在Dreamweaver中新建一个HTML页面，用你喜欢的排版方法，在页面上摆放下面这些东西：
  </p>

  <p>
    <a href="/media/legacy/2009/08/clip_image0035b45d.jpg" rel="WLPP"><img style="display:inline;border-width:0;" title="clip_image003" border="0" alt="clip_image003" src="/media/legacy/2009/08/clip_image0035b45d.jpg?w=300" width="552" height="70" /></a>
  </p>

  <p>
    我们这里可以简单些，如果没有前一页或者后一页了，不用隐藏前翻或后翻的链接。能够实现翻页、选择、删除功能就行了。注意用规范的命名方法为各个元素设置id。注意“更新”和“更旧”之间有一个div，用于显示当前页数和总页数，最下面有个div用于显示AJAX返回的列表。
  </p>

  <p>
    现在该给这些链接和按钮加上动作了。要实现AJAX请求翻页，应该有两个变量来存放当前页数和总页数。当前页数的默认值是1，总页数需要由服务器求得并返回给前台代码。翻页时，会将当前页数加或减后作为参数发给后台代码。当我们进行删除操作的时候，应该遍历所有列表项前面的复选框。这些复选框由服务器端的PHP代码生成，以数据库表中的记录的id（主键）作为复选框的id属性。所以当我们遍历到已选中的复选框时，就把它的id添加到删除列表selection变量中，以逗号分隔，最后把这个列表发给服务器，服务器就可以删除掉这些记录了。
  </p>

  <p>
    以下是客户端的JavaScript脚本：
  </p>

  <p>
    <a href="/media/legacy/2009/08/e7bfbbe9a1b5e5aea2e688b7e7abafe4bba3e7a0815b105d.png" rel="WLPP"><img style="border-bottom:0;border-left:0;display:inline;border-top:0;border-right:0;" title="翻页客户端代码" border="0" alt="翻页客户端代码" src="/media/legacy/2009/08/e7bfbbe9a1b5e5aea2e688b7e7abafe4bba3e7a0815b105d.png?w=114" width="552" height="1432" /></a>
  </p>

  <p>
    下一步是服务器代码。我们看到客户端JavaScript调用了两个PHP文件：helper_list_message.php和helper_del_list.php。
  </p>

  <p>
    <a href="/media/legacy/2009/08/e7bfbbe9a1b5e69c8de58aa1e599a8e7abafe4bba3e7a081efbc88e58897e8a1a8efbc895b55d.png" rel="WLPP"><img style="border-bottom:0;border-left:0;display:inline;border-top:0;border-right:0;" title="翻页服务器端代码（列表）" border="0" alt="翻页服务器端代码（列表）" src="/media/legacy/2009/08/e7bfbbe9a1b5e69c8de58aa1e599a8e7abafe4bba3e7a081efbc88e58897e8a1a8efbc895b55d.png?w=218" width="552" height="756" /></a>
  </p>

  <p>
    <a href="/media/legacy/2009/08/e7bfbbe9a1b5e69c8de58aa1e599a8e7abafe4bba3e7a081efbc88e588a0e999a4efbc895b75d.png" rel="WLPP"><img style="border-bottom:0;border-left:0;display:inline;border-top:0;border-right:0;" title="翻页服务器端代码（删除）" border="0" alt="翻页服务器端代码（删除）" src="/media/legacy/2009/08/e7bfbbe9a1b5e69c8de58aa1e599a8e7abafe4bba3e7a081efbc88e588a0e999a4efbc895b75d.png?w=300" width="489" height="267" /></a>
  </p>

  <p>
    你完全可以把列表代码和删除代码放在同一个文件里，用不同的do参数分别调用。
  </p>

  <p>
    修改服务器端的列表代码，试试让你的控件读取你数据库中的数据，看看是不是能像Gmail那样翻页？
  </p>

  <p>
    你可以添加一些功能，例如在用户单击了翻页链接后，给用户一个提示，表示正在与服务器通讯，以免响应时间过长时使用户感到困惑；还可以添加一个输入框，让用户输入页码并直接跳转过去，等等。Use your imagination.
  </p></p> </p>
</div>
