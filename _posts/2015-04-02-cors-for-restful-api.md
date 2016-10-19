---
title: 为 RESTful API 配置 CORS 实现跨域请求
quote: 利用 Ruby on Rails 可以很方便地实现 RESTful API，但如果我们需要通过 AJAX 跨域调用的话，还需要给 API 配置好 CORS 才行
author: James Shih
layout: post
permalink: /2015/04/02/cors-for-restful-api/
categories:
  - Web 开发
tags:
  - Ruby on Rails
  - RESTful
  - CORS
---
利用 Ruby on Rails 可以很方便地实现 [RESTful](https://en.wikipedia.org/wiki/Restful) API，但如果我们需要通过 AJAX 跨域调用的话，怎么办？

说到 AJAX 跨域，很多人最先想到的是 [JSONP](https://en.wikipedia.org/wiki/JSONP)。的确，JSONP 我们已经十分熟悉，也使用了多年，从本质上讲，JSONP 的原理是给页面注入一个 `<script>`，把远程 JavaScript 放在页面上执行。这种做法会带来一个显而易见的问题：如果调用的来源被攻击或篡改，那什么东西都可以注入到页面里，造成 [XSS](https://en.wikipedia.org/wiki/Cross-Site_scripting) 漏洞。另外，JSONP 本质上已经不是 [XMLHttpRequest](https://en.wikipedia.org/wiki/XMLHttpRequest)，所以在错误处理上也没有什么选择。而且 JSONP 只支持 GET 请求，所以 RESTful API 就没办法了。

这也就是为什么我们需要 [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)。CORS 是 Cross Origin Resource Sharing 的缩写，定义了浏览器和服务器间共享内容的新方式，通过它浏览器和服务器可以安全地进行跨域访问，它是 JSONP 的现代继任者。服务器上的 CORS 配置可以精细地指定允许跨域访问的条件：来源域、HTTP 方法、请求头、内容类型……等等。并且，CORS 让 XMLHttpRequest 也可以跨域，我们可以像往常一样编写 AJAX 调用代码。

[所有现代浏览器都支持 CORS](http://caniuse.com/#feat=cors)，所以你应该可以放心地使用它，只有在需要兼容老旧浏览器的场合，才用 JSONP 做 fallback。

支持 CORS 的浏览器在尝试进行跨域 XMLHttpRequest 时，会先发出一个“事前检查”，就是一个 `OPTIONS` 请求，其中会包括一些有用的请求头：

	Access-Controll-Request-Headers: accept, content-type
	Access-Controll-Request-Method: POST

接着服务器会做出响应：

	Access-Control-Allow-Origin: *
	Access-Control-Allow-Methods: POST, GET, OPTIONS
	Access-Control-Allow-Headers: X-Requested-With, Content-Type, Accept
	Access-Control-Max-Age: 1728000

最后浏览器会根据服务器的响应头，判断请求是否在服务器规定的范围内。比如来源是否在 `Access-Control-Allow-Origin` 里，HTTP 方法是不是在 `Access-Control-Allow-Methods` 里面，有没有不在 Allow-Headers 里面的请求头。如果以上条件都符合，那么浏览器就会放行这次请求，并且在 `Access-Control-Max-Age` 指定的时间内（单位是秒，以上设置的是 20 天）不需要再进行这种“事前检查”。

在以上服务器响应头中，`Access-Control-Allow-Headers` 不可以使用通配符。所以如果你要允许所有请求头，不妨把浏览器发来的 `Access-Control-Request-Headers` 直接返回。

事实上，如果跨域请求是“简单请求”，也就是 HTTP 方法为 `GET`、`HEAD`、`POST`，请求体的 MIME Type 是以下其中一种：`application/x-www-form-urlencoded`、`multipart/form-data` 或者 `text/plain`，并且没有自定义的请求头。这时浏览器只根据请求头中的 `Origin` 和服务器返回的 `Access-Control-Allow-Origin` 就可以判断了。但我们是 RESTful API，请求体是 `application/json`，所以只能用上面那种“事前检查”的方式。

另外，利用 CORS 还可以在跨域请求中发送 Cookie，这个特性是很有用的。只需要为 XMLHttpRequest 对象设置 withCredentials 属性：

```javascript
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```

但这种情况下，就不能指定 `Access-Control-Allow-Origin: *`，而是必须指定一个来源，比如 `http://mydomain.com`。

下面来说说在服务器端怎么配置，以 Rails 框架为例。注意：这只是一个很简单的例子，为了展示其原理，建议只在安全要求不高的项目上使用这种方法。Rails 有专门的 [Rack CORS](https://github.com/cyu/rack-cors) 中间件可以处理这个问题。

在一个 Controller（或者 `application_controller.rb` ）中添加 `before_filter` 和 `after_filter`。前者用来回应浏览器的“事前检查”，如果浏览器发来了 `OPTIONS` 请求，则返回一些响应头，并结束处理；后者则用来给响应添加 CORS 的响应头：

```ruby
# some_controller.rb

before_filter :cors_preflight_check
after_filter :cors_set_headers

# ...

def cors_preflight_check
  if request.method == 'OPTIONS'
    headers['Access-Controll-Allow-Origin'] = '*'
    headers['Access-Controll-Allow-Methods'] = 'POST, GET, OPTIONS'
    headers['Access-Controll-Allow-Headers'] = 'X-Requested-With, Content-Type, Accept'
    headers['Access-Controll-Max-Age'] = '1728000'
    render :text => '', :content-type => 'text/plain'
  end
end

def cors_set_headers
  headers['Access-Controll-Allow-Origin'] = '*'
  headers['Access-Controll-Allow-Methods'] = 'POST, GET, OPTIONS'
  headers['Access-Controll-Max-Age'] = '1728000'
end
```

最后，别忘了在 `routes.rb` 中允许 `OPTIONS` 请求：

```ruby
match 'controller', to: 'controller#action', via: [:options] ＃ 添加此行
resources :controller
```
