---
title: 指定使用带 www 的或不带 www 的域名
author: James Shih
layout: post
permalink: /2011/11/24/enforce-with-www-or-without-www/
categories:
  - Web Server
  - Web 开发
tags:
  - Apache
  - domain
  - Nginx
  - SEO
  - URL Rewrite
  - URL重写
  - 域名
---
留意过 SEO 方面的朋友应该了解，前面带有 www 的域名和不带 www 的域名是不同的。如果只解析了其中一个，通过另一个就无法访问到网站。一般来说，为了改善用户体验，应该将两个域名都做解析，让用户无论用哪个都能访问网站。

但是这样一来，在搜索引擎看来，domain.com 和 www.domain.com 变成了一模一样的两个网站，其收录的内容也会被分为两半。在 SEO 的意义上，这十分有害，等于两兄弟相斗，降低网站的权重。所以站长一般会选择其中一个域名作为标准，而将另一个域名以 301 Permanent 永久重定向至标准域名。

<!--more-->

例如，我们访问 amazon.cn，就会被自动转向 www.amazon.cn。这样既照顾到 SEO，又提升了用户体验。要做到这点，只需为网站根目录添加以下 URL 重写规则：

**Nginx**

<pre># 使用带 www 的域名
if ($host !~* ^www\.) {
    rewrite ^/(.*)$ $scheme://www.$host/$1 permanent;
}

# 使用不带 www 的域名
if ($host ~* ^www\.(.*)) {
    set $host_without_www $1;
    rewrite ^/(.*)$ $scheme://$host_without_www/$1 permanent;
}
</pre>

**Apache**

<pre># 使用带 www 的域名
RewriteEngine On
RewriteCond %{HTTP_HOST} !^www\.$ [NC]
RewriteRule ^(.*)$ http://www.mysite.com/$1 [L,R=301]

# 使用不带 www 的域名
RewriteEngine On
RewriteCond %{HTTP_HOST} ^www\.$ [NC]
RewriteRule ^(.*)$ http://mysite.com/$1 [L,R=301]
</pre>