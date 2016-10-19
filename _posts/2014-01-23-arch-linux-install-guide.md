---
title: Arch Linux 安装指引
author: James Shih
layout: post
permalink: /2014/01/23/arch-linux-install-guide/
authorsure_include_css:
  - 
categories:
  - Linux
tags:
  - Arch Linux
  - 操作系统安装
---
我喜欢 Arch Linux 的理由：

<ol style="list-style-type: decimal">
  <li>
    简洁、可定制度高：对于有系统洁癖的人，或者控制欲极强的人，Arch 才是你的 Linux
  </li>
  <li>
    包够新：官方源的软件包版本新得吓人，适合我这种尝鲜党
  </li>
  <li>
    系统滚动更新：也就是说，没有大版本号，从今天开始，pacman 让你永不落伍
  </li>
  <li>
    能学到更多知识：从来不知道 Linux 安装时干了些啥，直到我<del>膝盖中了一箭</del>用上了 Arch
  </li>
  <li>
    <a href="https://wiki.archlinux.org/index.php/Arch_Build_System">Arch Build System</a> 和 <a href="https://wiki.archlinux.org/index.php/Arch_User_Repository">Arch User Repository</a>
  </li>
</ol>

以下是我安装 Arch Linux 的过程，记录备参。

<!--more-->

### 制作安装媒介

首先到[这里][1]下载最新的官方 Arch Linux 安装媒介。然后，可以选择刻录或者写入到 USB 闪存盘或存储卡。

利用 dd 命令可以将安装媒介写入 USB 闪存盘或存储卡，参见[这里][2]：

    $ dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx && sync

完成后，用安装媒介启动电脑。

### 安装

#### 连接网络

安装程序已经自动启动了 `dhcpcd`，可以自动连接动态 IP 的有线网络。利用 ping 命令检查网络是否已连接成功：

    $ ping -c 3 www.google.com

如果是静态 IP 的有线网络或 WiFi，请参见[这里][3]。

#### 准备存储设备

##### UEFI

推荐使用 GPT 分区表，可以和 64 位 Windows 7 以上版本进行双系统启动。

利用 `cgdisk` 创建 GPT 分区：

    $ cgdisk /dev/sda

分区方案：

<table>
  <tr class="header">
    <th align="left">
    </th>
    
    <th align="left">
      类型
    </th>
    
    <th align="left">
      大小
    </th>
    
    <th align="left">
      类型代码
    </th>
  </tr>
  
  <tr class="odd">
    <td align="left">
      /dev/sda1
    </td>
    
    <td align="left">
      EFI 系统分区
    </td>
    
    <td align="left">
      512M
    </td>
    
    <td align="left">
      ef00
    </td>
  </tr>
  
  <tr class="even">
    <td align="left">
      /dev/sda2
    </td>
    
    <td align="left">
      Linux ext4
    </td>
    
    <td align="left">
      任意
    </td>
    
    <td align="left">
      8300
    </td>
  </tr>
  
  <tr class="odd">
    <td align="left">
      /dev/sda3
    </td>
    
    <td align="left">
      Linux 交换分区
    </td>
    
    <td align="left">
      <a href="https://wiki.archlinux.org/index.php/Swap">适量</a>
    </td>
    
    <td align="left">
      8200
    </td>
  </tr>
</table>

如果电脑上已经预装了 Windows 8，你需要：

<ol style="list-style-type: decimal">
  <li>
    关闭 Secure Boot
  </li>
  <li>
    EFI 系统分区已经存在，不要再创建
  </li>
</ol>

格式化分区，启用交换分区：

    $ mkfs.vfat -F32 /dev/sda1
    $ mkfs.ext4 /dev/sda2
    $ mkswap /dev/sda3
    $ swapon /dev/sda3

挂载分区：

    $ mount /dev/sda2 /mnt
    $ mkdir -p /mnt/home /mnt/boot
    $ mount /dev/sda1 /mnt/boot

##### BIOS

推荐使用 MBR 分区表，可以和所有 Windows 版本进行双系统启动。

利用 `fdisk` 创建 MBR 分区：

    $ fdisk /dev/sda

分区方案：

<table>
  <tr class="header">
    <th align="left">
    </th>
    
    <th align="left">
      类型
    </th>
    
    <th align="left">
      大小
    </th>
    
    <th align="left">
      类型代码
    </th>
  </tr>
  
  <tr class="odd">
    <td align="left">
      /dev/sda1
    </td>
    
    <td align="left">
      Linux ext4
    </td>
    
    <td align="left">
      任意
    </td>
    
    <td align="left">
      8300
    </td>
  </tr>
  
  <tr class="even">
    <td align="left">
      /dev/sda2
    </td>
    
    <td align="left">
      Linux 交换分区
    </td>
    
    <td align="left">
      <a href="https://wiki.archlinux.org/index.php/Swap">适量</a>
    </td>
    
    <td align="left">
      8200
    </td>
  </tr>
</table>

格式化分区，启用交换分区：

    $ mkfs.ext4 /dev/sda1
    $ mkswap /dev/sda2
    $ swapon /dev/sda2

挂载分区：

    $ mount /dev/sda1 /mnt
    $ mkdir -p /mnt/home

#### 选择软件源

编辑 `/etc/pacman.d/mirrorlist`，将要使用的源复制到最前面，然后刷新：

    $ pacman -Syy

#### 安装基本系统

利用 `pacstrap` 脚本安装基本系统：

    $ pacstrap /mnt base base-devel

#### 生成 fstab

利用 `genfstab` 生成 fstab：

    $ genfstab -U -p /mnt >> /mnt/etc/fstab
    $ cat /mnt/etc/fstab

注意，生成 fstab 后，务必检查一下内容是否正确。如果有问题，不要再运行 `genfstab`，直接手动编辑 `/mnt/etc/fstab` 文件。

### Chroot 到新系统

现在，新安装的系统还需要一些配置工作，才能自力引导系统。下面要 chroot 到新系统：

    $ arch-chroot /mnt /bin/bash

#### 语言地区

编辑 `/etc/locale.gen`，去掉以下项目的注释：

    en_US.UTF-8 UTF-8
    zh_CN.GB18030 GB18030
    zh_CN.GBK GBK
    zh_CN.UTF-8 UTF-8
    zh_CN GB2312

然后运行：

    $ locale-gen

最后，编辑 `/etc/locale.conf`：

    LANG=en_US.UTF-8

注意：不要在这里把系统全局设置成中文，因为终端无法显示中文，请单独将桌面环境设置为中文。

#### 终端字体（可选）

编辑 `/etc/vconsole.conf`：

    KEYMAP=us
    FONT=Lat2-Terminus16

#### 时区

将 `/etc/localtime` 软链接到 `/usr/share/zoneinfo/Zone/SubZone`。其中 `Zone` 和 `Subzone` 替换为所在时区，例如：

    $ ln -s /usr/share/zoneinfo/Asia/Chongqing /etc/localtime

#### 时间

设置系统时间：

    $ date -s "Jan 23, 2014 12:23:04"

或利用 ntp 自动校时：

    $ pacman -S ntp
    $ ntpdate time.nist.gov

将系统时间写入硬件时钟：

    $ hwclock --systohc --utc

注意：Linux 的硬件时钟使用 UTC，而 Windows 则使用本地时间。如果要和 Windows 进行双系统启动，可以将 Windows 设置为使用 UTC 硬件时钟，参见[这里][4]。同时关闭 Windows 上的自动校时功能，只用 Linux 上的 ntp。

#### 主机名（可选）

    $ echo myhostname > /etc/hostname

#### 连接网络

可以参照上面的方法连接网络。只不过，这次需要手动启用 dhcpcd 服务：

    $ systemctl enable dhcpcd.service

注意，GNOME 自带了 NetworkManager 来管理网络，这会与 dhcpcd 冲突，所以安装 GNOME 后要记得要切换到 NetworkManager：

    $ systemctl disable dhcpcd.service
    $ systemctl enable NetworkManager.service

#### 设置 Root 密码、添加用户

除了设置 Root 密码外，添加一个管理员组的帐户做平时使用：

    $ passwd
    $ useradd -G wheel user
    $ passwd user
    $ chfn user
    $ mkdir -m 700 /home/user
    $ chown user:user /home/user

#### 安装和配置 Bootloader

##### UEFI

    $ mount -t efivarfs efivarfs /sys/firmware/efi/efivars
    $ pacman -S grub os-prober efibootmgr
    $ grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch_grub --recheck
    $ grub-mkconfig -o /boot/grub/grub.cfg

##### BIOS

    $ pacman -S grub os-prober
    $ grub-install --target=i386-pc --recheck /dev/sda
    $ grub-mkconfig -o /boot/grub/grub.cfg

#### 卸载分区并重启系统

现在新系统已经准备好第一次引导了，先退出 chroot 环境：

    $ exit

然后，卸载分区并重启系统：

    $ umount -R /mnt
    $ reboot

### 在新系统上安装需要的软件

#### 安装 X 窗口管理系统

    $ pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils

#### 显卡驱动

查询显卡类型：

    $ lspci | grep VGA

查询可用的开源驱动：

    $ pacman -Ss xf86-video | less

安装通用驱动（vesa）：

    $ pacman -S xf86-video-vesa

支持硬件加速的驱动程序可以在安装 X 时自动提示你安装，只需要选择正确的显卡类型，不需要显式安装。

#### 声卡驱动

通常不需要配置就能工作，只需解除静音。需要做的只是安装 alsa-utils 软件包：

    $ pacman -S alsa-utils

#### 输入设备驱动

输入设备驱动已经在安装 X 时自动安装，一般不需要显式安装。不过，笔记本或触摸屏用户需要安装 synaptics：

    $ pacman -S xf86-input-synaptics

#### 如果是在 VMware 中安装

安装一些专门用于虚拟机的驱动程序：

    $ pacman -S xf86-input-vmmouse xf86-video-vmware svga-dri

安装 VMware Tools，在 VMware 中选择“安装 VMware Tools”，然后：

    $ pacman -S linux-headers
    $ for x in {0..6}; do mkdir -pv /etc/init.d/rc$x.d; done
    $ mount /dev/cdrom /mnt
    $ cd /root
    $ tar zxf /mnt/VMwareTools*.tar.gz
    $ cd vmware-tools-distrib
    $ ./vmware-install.pl

按提示一步步进行，如果最后报错，一般也不用担心，重启后 VMware Tools 能正确启动。

#### 安装字体

安装一些常用的中英文字体：

    $ pacman -S ttf-dejavu wqy-zenhei wqy-microhei

#### 安装桌面环境

这里以 GNOME 为例：

    $ pacman -S gnome

要启用图形界面登录和锁屏等功能，可启用 gdm：

    $ systemctl enable gdm.service

安装 GNOME 的一些常用工具（更多工具和扩展请参见[这里][5]）：

    $ pacman -S gnome-tweak-tool gnome-packagekit gnome-settings-daemon-updates

安装中文输入法，GNOME 集成了 iBus，所以只需安装合适的输入法引擎，然后在 GNOME 系统设置中启用即可，参见[这里][6]：

    $ pacman -S ibus-libpinyin

### 完成安装

重新启动，即可由图形界面登录到系统了。祝贺你！说实话，你能有足够的耐心做到这里而没有叛逃到 Debian/CentOS，我很欣慰。相信你很快就会意识到这些麻烦不是没有回报的，欢迎加入 Archers 的行列！

如有任何问题，RTFM：[Arch Linux Beginners&#8217; Guide][7]

### 一些优化

#### SATA 启用 AHCI 模式

SATA 有两种工作模式：原生的 [AHCI][8] 模式提供更好的性能（如热插拔和 [NCQ][9] 支持）、模拟的 IDE 模式提供更好的兼容性。一般主板出厂默认将 SATA 模式设置为 IDE 模式，但如今先进的 Linux 和 Windows 都早已原生支持 AHCI，所以我们最好打开 AHCI 模式以优化性能。

Arch Linux 在安装好以后，内核镜像默认没有载入 AHCI 驱动模块。修改 `/etc/mkinitcpio.conf`，添加 `ahci` 到 `MODULES` 变量：

    MODULES="ahci"

然后重建内核镜像，重新启动后 AHCI 驱动就会加载：

    $ mkinitcpio -p linux

在主板 UEFI 或 BIOS 中，将 SATA 模式从 `IDE`（或 `PATA Emulation` 等等），设置为 `AHCI`（或 `Native` 等等）。需要注意的是，如果你还在用 Windows XP，它需要安装 AHCI 驱动才行。Windows Vista 及以后的版本则不需要担心这个问题（但如果你是在 Windows 安装完成后才启用 AHCI 模式，因为安装期间 Windows 会自动禁用未使用的存储驱动程序，你需要参考 [KB922976][10]（Windows Vista/7）或 [KB2751461][11]（Windows 8）来启用 AHCI 驱动程序）。

设置好以后，你可以从 `dmesg` 命令的输出里，找到 AHCI 和 NCQ 成功启用的证据：

    $ dmesg
    ...
    SCSI subsystem initialized
    libata version 3.00 loaded.
    ahci 0000:00:1f.2: version 3.0
    ahci 0000:00:1f.2: irq 24 for MSI/MSI-X
    ahci 0000:00:1f.2: AHCI 0001.0300 32 slots 6 ports 6 Gbps 0x10 impl SATA mode
    ahci 0000:00:1f.2: flags: 64bit ncq led clo pio slum part ems apst 
    scsi host0: ahci
    scsi host1: ahci
    scsi host2: ahci
    scsi host3: ahci
    scsi host4: ahci
    scsi host5: ahci
    ...
    ata5.00: 976773168 sectors, multi 16: LBA48 NCQ (depth 31/32), AA
    ...

#### 优化系统启动速度

Arch Linux 的 `systemd-analyze` 是个很不错的工具，利用它你可以很直观地观察到系统启动的时间都花到哪儿去了：

    $ systemd-analyze
    Startup finished in 6.857s (firmware) + 3.157s (loader) + 1.870s (kernel) + 8.157s (userspace) = 20.044s

我注意到打开 AHCI 后，内核和用户空间的载入速度明显提高了，总启动时间从约 30 秒缩短到 20 秒，效果非常明显。

用下面这个命令，可以了解到是什么东西启动最慢：

    $ systemd-analyze blame
    
此外，还可以把启动过程绘制成 SVG 图表供你审阅（用 GNOME 的图片预览或 Chrome 浏览器都可以打开），这个图表中你还可以观察到是否有启动慢的组件影响到了依赖它的组件的启动：

    $ systemd-analyze plot > plot.svg

 [1]: https://www.archlinux.org/download/
 [2]: https://wiki.archlinux.org/index.php/USB_Installation_Media
 [3]: https://wiki.archlinux.org/index.php/Beginners%27_Guide#Establish_an_internet_connection
 [4]: https://wiki.archlinux.org/index.php/Time#UTC_in_Windows
 [5]: https://wiki.archlinux.org/index.php/GNOME
 [6]: https://wiki.archlinux.org/index.php/IBus
 [7]: https://wiki.archlinux.org/index.php/Beginners%27_Guide
 [8]: https://en.wikipedia.org/wiki/Advanced_Host_Controller_Interface
 [9]: https://en.wikipedia.org/wiki/Native_Command_Queuing
 [10]: https://support.microsoft.com/kb/922976
 [11]: https://support.microsoft.com/kb/2751461

 *[RTFM]: Read the Fucking Manual