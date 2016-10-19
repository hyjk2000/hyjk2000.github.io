---
title: 怎样在 Windows 上开发 Safari 扩展
author: James Shih
layout: post
permalink: /2013/04/25/develop-safari-extension-on-windows/
categories:
  - Apple
  - PC
  - Web 开发
  - 开发工具
tags:
  - certification
  - extension
  - Safari
  - Windows
  - 扩展
  - 证书
---
最近开始对开发浏览器扩展产生了浓厚的兴趣。这几天在工作中需要开发一个 Safari 扩展，我们部门的电脑却都是 Windows PC。接触过 Safari 扩展开发的同学应该都知道，要想开发 Safari 扩展，必须注册 Safari Developer Program 并取得开发证书才行。之前我在家用的是 Mac，按照 Safari Certificates Manager 的说明，很容易就把开发证书安装好了。但是苹果对于在 Windows 上如何申请和安装开发证书却没作任何说明，所以只能另想办法，否则我就只有找别的部门有 Mac 的人帮我做了。

经过一顿 Google 和反复试验，我还是在 Windows 上成功地装好了开发证书。途径是，用一个叫 [XCA][1] 的小工具代替 Mac 上的钥匙串访问。

思路明确了，可是过程还是有点曲折，待我细细道来。

<!--more-->

### Step 0：注册 Safari Developer Program

这一步不用多说了，传送门：<https://developer.apple.com/programs/safari/>，目前是免费的。

### Step 1：安装 XCA for Windows

到 <http://sourceforge.net/projects/xca/> 下载最新的 XCA for Windows 并安装。值得一提的是，这货也有 Mac 和 Linux 版。只不过 Mac 上用不着，Linux 上没有 Safari，所以对我来说暂时没啥用。

### Step 2：在 XCA 中生成私钥（Private Key）和证书签名申请（Certificate Signing Request）

打开 XCA，按 Ctrl+N 新建一个数据库。接着在 **Private Key** 选项卡中单击 **New Key**。在对话框中随便起一个名字，生成一个长度 2048 bit 的 RSA 类型的私钥。

<figure>
  <img src="/media/legacy/2013/04/20130425_develop-safari-extension-on-windows_1.png" alt="20130425_develop-safari-extension-on-windows_1">
</figure>

在 **Certificate signing request** 选项卡中单击 **New Request**。在对话框的 **Subject** 选项卡中填写一些信息，**Internal Name** 是必填的。这些信息会存放进生成的 CSR 文件中，并最终存入申请到的开发证书。

### Step 3：导出刚刚生成的 Certificate Signing Request，用它申请开发证书

选择刚生成的 Request，单击 **Export** 导出，导出格式选择 **PEM**。接着，到 [这里][2] 上传导出的文件。稍等一会儿，就可以下载申请好的开发证书了。

### Step 4：将私钥加入开发证书

从苹果下载的开发证书如果直接导入，Safari 仍然无法识别。原因是导入的证书中没有私钥。于是我们再次祭出 XCA，在 **Certificates** 选项卡导入从苹果下载的开发证书（.cer）。接着再单击 **Export** 导出，导出格式选择 **PKCS #12**（.p12）。

<figure>
  <img src="/media/legacy/2013/04/20130425_develop-safari-extension-on-windows_2.png" alt="20130425_develop-safari-extension-on-windows_2">
</figure>

需要注意的是，如果在 **Private Key** 选项卡下没有私钥，你需要生成或导入一个。否则导出的 PKCS #12 格式的证书中没有私钥，无法使用。

### Step 5：将 PKCS #12 格式的证书导入 Windows

双击刚刚导出的 PKCS #12 格式的证书，在 **证书导入向导** 中导入证书，让 Windows 自动选择证书存储位置。

按 **WinKey+R** 打开 **运行**，输入 **certmgr.msc** 打开证书管理器。在 **个人** 下可以找到刚刚导入的证书，说明一切正常。

由于苹果似乎停止了 Safari for Windows 的开发，最新的 Safari 6 没有 Windows 版。目前在 Windows 上最新的版本是 [Safari 5.1.7][3]，所以暂时只能用这个版本进行开发。测试的话，为了稳妥起见，还是得老老实实找一台 Mac，在 Safari 6 上测试。

在 Safari 偏好设置的 **高级** 选项卡中选中 **在菜单栏中显示“开发”菜单**。在页面菜单中选择 **开发** -> **显示扩展创建器**。新建一个扩展，出现下图上半这样的，就说明你成功了。如果不幸出现下半那样的，再试试吧。

<figure>
  <img src="/media/legacy/2013/04/20130425_develop-safari-extension-on-windows_3.png" alt="20130425_develop-safari-extension-on-windows_3">
</figure>

说句题外话，做过 Chrome, Safari, Firefox, Opera 扩展的开发后，最喜欢的还是 Chrome 和 Safari。Chrome 的优点是开放、方便、JSON 格式的配置文件易写易读，Safari 的优点是有证书保护更安全，而且扩展创建器用起来很方便。Opera 用了 [W3C Widget][4] 标准的 XML 配置文件，也给我留下了深刻印象。Firefox 则稍显麻烦，至于 Internet Explorer …… 我就不说什么了，虽然我没亲手去做，但我知道它相当令人抓狂。

 [1]: http://sourceforge.net/projects/xca/
 [2]: https://developer.apple.com/account/safari/certificate/certificateRequest.action
 [3]: http://support.apple.com/kb/DL1531
 [4]: http://www.w3.org/TR/widgets/

 *[XCA]: X Certificate and Key management
