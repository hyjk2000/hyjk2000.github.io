---
title: 为 Sublime Text 2 安装 Emmet (Zen Coding)
author: James Shih
layout: post
permalink: /2012/08/31/install-zen-coding-for-sublime-text-2/
categories:
  - Web 开发
  - 开发工具
tags:
  - CSS
  - Emmet
  - HTML
  - OS X
  - Sublime Text
  - Zen Coding
---
Zen Coding 是一款帮助 Web 前端工程师快速编写 HTML 和 CSS 的利器。直到昨天我无意中看到一个 Zen Coding 的演示视频并大为震惊以后，我才决定开始使用，实在是有点火星了。想想以前没有 Zen Coding 的日子，不堪回首啊。

有了 Zen Coding，你碰到下面这种代码，就不会觉得头痛了：

```html
<select name="month" id="month">
  <option value="1">1月</option>
  <option value="2">2月</option>
  <option value="3">3月</option>
  <option value="4">4月</option>
  <option value="5">5月</option>
  <option value="6">6月</option>
  <option value="7">7月</option>
  <option value="8">8月</option>
  <option value="9">9月</option>
  <option value="10">10月</option>
  <option value="11">11月</option>
  <option value="12">12月</option>
</select>
```

你只需要输入：

```css
select#month[name=month]>option[value="$"]*12>{$月}
```

很吸引人不是么？下面就来讲一讲在 Sublime Text 2 上安装 Zen Coding 的方法。

<!--more-->

**1. 安装 Package Control**

Package Control 是 Sublime Text 2 的插件管理器，我们需要它来安装很多有用的插件，例如 Zen Coding。按 <kbd title="Control + `">⌃`</kbd> 打开控制台，输入以下命令，按 <kbd title="Return">⏎</kbd>：

```python
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'
```

很快安装就会完成，按 <kbd title="Command + Q">⌘Q</kbd> 退出再重新启动。

**2. 安装 Zen Coding**

按 <kbd title="Shift + Command + P">⇧⌘P</kbd> 打开命令面板，输入命令 install package，按 <kbd title="Return">⏎</kbd>。这时下方会出现一个插件列表，输入 <del datetime="2012-11-15T06:21:47+00:00">zencoding</del> emmet，在下方列表中选择 <del datetime="2012-11-15T06:21:47+00:00">ZenCoding</del> Emmet 即可。

**Update (11/15/2012): **Zen Coding 现已改名 Emmet，原安装方法将无法继续使用，请输入 emmet 来安装。若你已经安装了 Zen Coding，请先在命令面板中输入 remove package，移除原来的 Zen Coding，再重新安装 Emmet。

**3. 激活 Zen Coding 缩写模式**

在 HTML 文档中（新建文档是不行的），按 <kbd title="Control + Option + Return">⌃⌥⏎</kbd> 激活 Zen Coding 缩写模式。在底部的框中输入缩写，输出效果会直接在光标处呈现。

要了解更多 Zen Coding 缩写的语法，请参考：[How to Use Zen Coding][1]
要为 Notepad++、Dreamweaver 等安装 Zen Coding，请参考：[Zen Coding Downloads][2]

 [1]: http://www.emeditor.com/modules/feature1/rewrite/tc_37.html
 [2]: http://code.google.com/p/zen-coding/downloads/list
