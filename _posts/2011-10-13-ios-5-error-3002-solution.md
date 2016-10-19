---
title: 更新 iOS 5 出现“未知错误 (3002)”的解决方法
author: James Shih
layout: post
permalink: /2011/10/13/ios-5-error-3002-solution/
categories:
  - Gadgets
tags:
  - iOS
  - iOS 5
  - unknown error
  - update
  - 更新
  - 未知错误
---
<img src="/media/legacy/2011/10/20111013_ios_5.png" alt="20111013_ios_5">

有不少朋友反映，自己在为爱机更新 iOS 5 时遇到“未知错误 (3002)”，而导致无法更新。今天我在更新 iPad 的时候，也遇到了同样的问题。但是，我发现这和更新步骤有关。

由于在国内，用 iTunes 直接下载更新包速度非常慢，而且容易下载失败。大部分朋友都是用下载工具从苹果官网下载更新包来更新的。然后，在 iTunes 中，按住 Shift 键点击“更新”按钮，再选择刚才下载的更新包。

此次 iOS 5 更新时，使用该方法就可能会遇到“未知错误 (3002)”。我注意到，通过直接选择更新包，似乎跳过了 iTunes 和苹果更新服务器的联系过程。为了让更新步骤更规范，我选择将下载到的更新包移动到 iTunes 直接下载时存放到的位置。这样，iTunes 会认为更新包是自己下载的，于是问题便顺利解决。

<!--more-->

具体步骤如下：

1.下载 iOS 5 离线更新包

2.将更新包移动到下列位置：

	\Users\[用户名]\AppData\Roaming\Apple Computer\iTunes\[iPhone/iPad/iPod] Software Updates (Windows 7/Vista)

	\Documents and Settings\[用户名]\Application Data\Apple Computer\iTunes\[iPhone/iPad/iPod] Software Updates (Windows XP)

	~/Library/iTunes/[iPhone/iPad/iPod] Software Updates (Mac)

3.打开 iTunes，连接 iOS 设备，直接点击“更新”按钮。

**注意**：此解决方法针对在更新中出现“未知错误 (3002)”的用户，并不能保证解决其他类型的错误。

**附 iOS 5 离线更新包下载地址：**

**iPhone**

*   [iPhone 3GS][1]
*   [iPhone 4 GSM][2]
*   [iPhone 4 CDMA][3]

**iPad**

*   [iPad][4]
*   [iPad 2 WiFi][5]
*   [iPad 2 3G][6]
*   [iPad 2 CDMA][7]

**iPod touch**

*   [iPod touch 3代][8]
*   [iPod touch 4代][9]

 [1]: http://appldnld.apple.com/iPhone4/041-8356.20111012.SQRDT/iPhone2,1_5.0_9A334_Restore.ipsw
 [2]: http://appldnld.apple.com/iPhone4/041-8358.20111012.FFc34/iPhone3,1_5.0_9A334_Restore.ipsw
 [3]: http://appldnld.apple.com/iPhone4/041-9743.20111012.vjhfp/iPhone3,3_5.0_9A334_Restore.ipsw
 [4]: http://appldnld.apple.com/iPhone4/041-8357.20111012.DTOrM/iPad1,1_5.0_9A334_Restore.ipsw
 [5]: http://appldnld.apple.com/iPhone4/041-9618.20111012.Zxb22/iPad2,1_5.0_9A334_Restore.ipsw
 [6]: http://appldnld.apple.com/iPhone4/041-9619.20111012.y34Nx/iPad2,2_5.0_9A334_Restore.ipsw
 [7]: http://appldnld.apple.com/iPhone4/041-9620.20111012.pnB4r/iPad2,3_5.0_9A334_Restore.ipsw
 [8]: http://appldnld.apple.com/iPhone4/061-8360.20111012.New3w/iPod3,1_5.0_9A334_Restore.ipsw
 [9]: http://appldnld.apple.com/iPhone4/061-9622.20111012.Evry3/iPod4,1_5.0_9A334_Restore.ipsw
