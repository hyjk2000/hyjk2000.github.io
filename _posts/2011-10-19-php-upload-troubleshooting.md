---
title: PHP 上传文件故障排除
author: James Shih
layout: post
permalink: /2011/10/19/php-upload-troubleshooting/
categories:
  - PHP
  - Web 开发
tags:
  - PHP
  - php.ini
  - troubleshooting
  - upload
  - 上传
  - 故障排除
---
PHP 文件上传时出现问题时，就要在表单、后台处理程序和服务器配置上找原因。最常见的故障原因有：PHP 配置不正确、服务器上的相关目录没有写权限、表单编写有问题等。

如果在使用 PHP 上传时遇到问题，请参阅以下故障排除步骤：

<!--more-->

**检查表单**

```html
<form id="form_1" action="upload.php" method="post" enctype="multipart/form-data">
  <input type="file" accept="text/plain" name="file">
  <input type="hidden" name="MAX_FILE_SIZE" value="4096">
  <input type="submit" value="Submit">
</form>
```

检查 method、enctype、MAX_FILE_SIZE 等是否正确，各表单项的 name 属性是否正确，另外 `<form>` 标签必须关闭，也不可以在任何时候用 JavaScript 禁用文件表单域。

**检查服务器上的目录权限**

确保服务器上的上传文件保存目录、上传临时目录（upload_tmp_dir）都有读写权限。而且服务器的磁盘空间充足。

对于 Windows，权限的问题有时会比较讨厌，需要这样设置：

上传临时文件目录（upload_tmp_dir）：应用程序池对应用户拥有其修改权限；IUSR（Internet 来宾帐号）拥有**其父目录的**列出文件夹内容权限。

所以，一个比较方便的策略是，在任意一个地方建立一个 tmp 文件夹，为其设置 IUSR 的列出文件夹内容权限和应用程序池对应用户的修改权限；然后在其下建立一个 upload 文件夹，任其继承 tmp 的权限。最后，将 session.save_path 设在 tmp，upload_tmp_dir 设在 upload。

如果由于权限问题，PHP 无法写入上传临时目录，它不会报错而是直接改用系统的临时目录（Linux 下的 /tmp 或者 Windows 下的 %SYSTEMROOT%\Temp），造成 upload_tmp_dir 被忽略或未生效的假象。

一般来讲，我们都会使用 open_basedir 来限制 PHP 可以访问的文件系统范围。这时我们需要注意将上传临时目录也包含在其中。

`open_basedir = "/home/wwwroot;/tmp"`

如果以上步骤没有解决问题，则需要检查 PHP 配置文件 php.ini：

**1. 启用上传**

`file_uploads = On`

**2. 设置上传文件大小限制**

`upload_max_filesize = 50M`

**3. 设置 POST 大小限制**

`post_max_size = 60M`

需要设置为比 upload_max_filesize 大一些。

**4. 设置 PHP 内存限制**

`memory_limit = 128M`

需要设置为比 post_max_size 大一些

**5. 设置 PHP 接受输入的时间限制**

`max_input_time = 60`

这个时间是指 PHP 处理输入数据所花的时间，而非用户上传文件所花的时间。确保设置为默认的 60 秒即可。

**6. 设置上传文件临时目录**

`upload_tmp_dir = '/tmp'`

此项可以留空，系统会自动使用默认的临时目录。但如果使用了 open_basedir 限制 PHP 可以访问的目录范围，则会阻止 PHP 访问系统临时目录，造成上传失败。在这种情况下必须指定 upload_tmp_dir。