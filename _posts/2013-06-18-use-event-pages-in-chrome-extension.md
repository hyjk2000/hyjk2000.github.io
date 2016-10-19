---
title: Chrome 扩展利用 Event Pages 减少后台资源占用
quote: 从 Chrome 22 开始支持的 Event Pages，可以自动检测我们注册的事件响应，仅在这些事件发生时才载入后台页面，处理完成后自动卸载，这样就能够有效地减少资源占用。
author: James Shih
layout: post
permalink: /2013/06/18/use-event-pages-in-chrome-extension/
categories:
  - Web 开发
tags:
  - background page
  - chrome
  - event page
  - extension
---
在 Chrome 扩展的开发过程中，我们经常要利用一个后台页面（background.html 或 background.js）来处理事件或消息。这个页面在插件安装并启用后会一直驻留在内存中，即使用户没有使用插件也是如此。如果用户在浏览器上安装了不少插件，这些插件的后台页面会浪费掉数量可观的内存，这也是 Chrome 资源占用庞大的肇因之一。

事实上，在很多情况下，后台页面都只是处理偶尔发生的事件，如用户访问到符合某些条件的网站或 URL 等等。处于闲置状态的插件却仍然占用大量的系统资源，这着实令人十分恼火，以至于用户只好尽量避免安装扩展。幸运的是，从 Chrome 22 开始支持的 [Event Pages][1]，可以自动检测我们注册的事件响应，仅在这些事件发生时才载入后台页面，处理完成后自动卸载，这样就能够有效地减少资源占用。

<!--more-->

最近，我写了一个试验性的从 Chrome Web Store 下载 crx 文件的扩展。这个扩展做的事情非常简单，就是检测到用户访问到某个商店项目的详细页面时，Omnibox 右侧就显示一个 [Page Action][2] 图标，用户点击这个图标即可下载 crx 文件。常规的做法是监听 [`chrome.tabs.onUpdated`][3] 事件，然后判断 URL 是否是商店项目的详细页面，如果是就从 URL 中提取项目的名称和 ID，生成下载链接供用户下载。这也就意味着，后台页面会一直驻留在内存中，检查用户浏览的所有 URL。这无论从资源占用的角度来看，还是用户隐私的角度来看，都很不理想。

以下是常规的做法（略去点击 Page Action 图标后显示的 popup.html，它的内容就是取得当前活动标签页的 URL 并加以分析，生成下载链接）：

*manifest.json*

```javascript
{
  "manifest_version": 2,
  "name": "GetCRX",
  "version": "1.0",
  "description": "Download .crx file of any Chrome extension",

  "icons": {
    "128": "icon128.png",
    "48": "icon48.png"
  },

  "background": {
    "scripts": [
      "background.js"
    ]
  },

  "page_action": {
    "default_icon": {
      "19": "icon19.png",
      "38": "icon38.png"
    },
    "default_title": "GetCRX",
    "default_popup": "popup.html"
},

  "permissions":[
    "tabs",
    "downloads",
    "https://clients2.google.com/service/update2/crx"
  ]
}
```

*background.js*

```javascript
function checkForValidUrl(tabId, changeInfo, tab) {
  if (tab.url.indexOf('https://chrome.google.com/webstore/detail/') === 0) {
    chrome.pageAction.show(tabId);
  }
}

chrome.tabs.onUpdated.addListener(checkForValidUrl);
```

现在，点击 Chrome 菜单中的 *显示后台网页*，可以看到 GetCRX 扩展的后台页面一直驻留在内存中，占用了 26.3MB 的内存。

<figure>
  <img src="/media/legacy/2013/06/20130618_chrome-extension-use-event-pages_1.png" alt="后台页面占用了 26.3MB 的内存">
</figure>

由于 GetCRX 扩展只有在用户浏览 Chrome Web Store 时才会工作，如果用户注意到它平时一直会占用如此多的内存，很可能会禁用或卸载掉它。为了让它在不工作时完全不占用资源，我决定改用 Event Pages。

首先，在 manifest.json 中，更改后台页面和要求的权限：

*manifest.json*

```javascript
{
	"manifest_version": 2,
	"name": "GetCRX",
	"version": "1.0",
	"description": "Download .crx file of any Chrome extension",

	"icons": {
		"128": "icon128.png",
		"48": "icon48.png"
	},

	"background": {
		"scripts": [
			"eventPage.js"
		],
		"persistent": false
	},

	"page_action": {
		"default_icon": {
			"19": "icon19.png",
			"38": "icon38.png"
		},
		"default_title": "GetCRX",
		"default_popup": "popup.html"
	},

	"permissions":[
		"tabs",
		"webNavigation",
		"downloads",
		"https://clients2.google.com/service/update2/crx"
	]
}
```

注意第 16 行的 `"persistent": false`，这个参数的缺省值为 true，也就是使后台页面一直驻留。设置为 `false` 后，Chrome 就可以视情况自动卸载它。

不过这里还有个问题：`chrome.tabs.onUpdate` 仍会在每一个标签页更新时触发，所以事实上后台页面还是会频繁地载入。我们需要在 URL 符合条件时才响应事件，也就是 [Event Filtering][4]，才能防止后台页面不必要的载入。不幸的是 `chrome.tabs.onUpdate` 事件并不支持 Event Filtering，所以这里需要改用 [`chrome.webNavigation.onCommitted`][5] 事件。于是我们在 `permissions` 中增加一个 `webNavigation`。然后把后台脚本写成这样：

*eventPage.js*

```javascript
(function() {
	var showPageAction = function(details) {
		chrome.pageAction.show(details.tabId);
	}

	var eventFilter = {url: [{urlPrefix: "https://chrome.google.com/webstore/detail/"}]};

	chrome.webNavigation.onCommitted.addListener(showPageAction, eventFilter);
})();
```

这样一来，Chrome 就明白我们只有当 URL 符合条件时才需要响应事件，所以平时就可以将后台页面卸载掉。

重新载入扩展，再查看后台页面，发现 GetCRX 扩展的后台页面在插件启用几秒钟后便自动卸载掉了。在访问维基百科时，后台页面也不会载入：

<figure>
  <img src="/media/legacy/2013/06/20130618_chrome-extension-use-event-pages_2.png" alt="利用 Event Pages 后，后台页面自动卸载，访问维基百科时也不会载入">
</figure>

只有在访问 Chrome Web Store 中某个 App 的详细页面时，后台页面才会自动载入。我在截图时甚至需要动作麻利，否则几秒钟后，后台页面就又被自动卸载了：

<figure>
  <img src="/media/legacy/2013/06/20130618_chrome-extension-use-event-pages_3.png" alt="访问符合条件的 URL 时，后台页面自动载入">
</figure>

事件响应成功执行，Omnibox 右侧正确显示出 Page Action 图标：

<figure>
  <img src="/media/legacy/2013/06/20130618_chrome-extension-use-event-pages_4.png" alt="20130618_chrome-extension-use-event-pages_4">
</figure>

这样，GetCRX 扩展就真正做到了“招之即来，挥之即去”。

不过，由于 Chrome Web Store 中页面的导航几乎全部是 AJAX 无刷新导航，这就导致 `chrome.webNavigation.onCommitted` 事件不会触发，只有 `chrome.tabs.onUpdate` 事件被触发。所以，在商店中点击一个项目后，无法正确显示出 Page Action 图标，需要刷新一下才能显示（这时才进行真正的“导航”动作）。所以只能希望未来 Google 会让 `chrome.tabs.onUpdate` 也支持 Event Filtering，或者我能想到更好的办法。尽管如此，这种做法对于一般的网站仍是适用的，所以我建议所有开发者都尽量利用 Event Pages 来实现事件处理以节省资源。

**Update (2013-06-24): **突然想到，下载 CRX 这种简单的任务，完全可以用 [bookmarklet][6] 来完成。创建一个书签，然后将以下代码填入 **URL** 一栏：

```javascript
javascript:(function(){var a=window.location.href;if(-1==a.indexOf("https://chrome.google.com/webstore/detail/"))alert("This page is not a Chrome Web Store item detail page.");else{var b=a.split("/detail/"),b=b[1].split("/"),b=b[0],b=b+".crx",a=a.split("/")[6],a=a.split("?"),a=a[0],a="https://clients2.google.com/service/update2/crx?response=redirect&x=id%253D"+a+"%2526uc",c=window.open(null,null,"top="+(screen.height/2-75-30)+",left="+(screen.width/2-150)+",width=300,height=150,toolbar=no").document;c.open();c.write('<html><head><style>body{font:12px/1.5em "Lucida Grande","Lucida Sans Unicode",sans-serif;min-width:160px;padding:30px;text-align:center;overflow:hidden}img{vertical-align:middle}h1{color:#666;font-size:28px}.file-name{color:#333;white-space:nowrap}button{font-size:18px}</style></head><body><h1>GetCRX</h1>');c.write('<a href="'+a+'" download="'+b+'" onclick="if(window.chrome){alert(\'Please Right Click & Save As ...\');return false}">'+b+"</a><br>(Right Click & Save As ...)");c.write("</body></html>")}})();
```

 [1]: http://developer.chrome.com/extensions/event_pages.html
 [2]: http://developer.chrome.com/extensions/pageAction.html
 [3]: http://developer.chrome.com/extensions/tabs.html#event-onUpdated
 [4]: http://developer.chrome.com/extensions/events.html#filtered
 [5]: http://developer.chrome.com/extensions/webNavigation.html#event-onCommitted
 [6]: http://zh.wikipedia.org/wiki/%E5%B0%8F%E4%B9%A6%E7%AD%BE
