---
title: NIXOS
date: 2025-05-15 13:39:01
cover: /img/nixos/6.png
categories: linux
tags:
    - linux distro
    - minimize installation
---

{% colorquote info %}
之前写过一次blog post是关于arch linux安装的，当时采用的是英文，花了2个小时一步一步跟着arch wiki的user guide，最终还是败给了假期在家实际安装的arch linux的HP laptop，主要原因可能是当时了解不够深导致自己产生了能够做好的错觉(雾，现在想了一下完全是当时只想着整花里胡哨(linux ricing)，导致自己一直都在错误地使用linux，现在打算回过头来以现在的理解，记录一下真正安装linux过程，其实这应当是一个有趣但容易忽视的过程。
{% endcolorquote %}

这里给出我玩了几个月linux ricing后的想法：

- linux ricing 本身可以驱动学习linux，但没必要。因为大部分时候只是在使用别人写好的上层软件，并不会触及深入的知识。
- linux 本身也是tool，如何运用高效的tool才是有意义的。
- linux distro不重要，能安装软件的os就好

{% colorquote warning %}
这里之前本来是想着记录arch linux的完整的安装记录，但是arch linux在我手里始终用顺手，所以花了几天入坑了nixos。发现nixos才是完美契合我使用的linux distro，因此这里改为nixos的安装，因为nixos有别于一般的linux distro的安装过程，而且需要大量前置学习相关理念，所以这篇文章主要面向对nixos感兴趣的人。
{% endcolorquote %}

话不多说，Let's do it!

> image

1) .iso系统镜像文件 from nixos官网
2) ventoy启动盘管理工具 from github
3) 使用ventoy直接管理nixos image

**details:** .iso文件寻求简单下载gui安装器，minimal版本只有cli。熟练linux，推荐minimal精细化控制系统

流程：download .iso image --> download ventoy in usb device --> copy .iso image file to usb device

> boot

1) bios直接启动
2) uefi在关闭secure boot后启动
3) 选择从usb设备启动
4) gui安装器按照步骤安装即可
5) 以下过程主要为cli介绍

**details:** 官方介绍为`nixos-help`可以直接在gui形式下显示安装教程，但是实际上在cli中没有作用，因为并没有x window server和w3m等会直接报错。

> network

1) 确保设备连接网络 -- iwctl, nmcli...
2) 安装必备的editor -- vim(按照个人习惯)

> console font

1) setfont ter-v32n -- 失效则未安装该console-font,这里不选择安装,因为不是很影响后续操作
2) setfont -d -- 2倍字体，可以临时解决内容太小问题

> partition and mount

1) uefi -> gpt分区 -- fdisk
2) bios -> mbr分区 -- fdisk
3) 分区完后使用mkfs格式化分区
4) 格式化后使用mount挂载到指定的挂载点，如：/boot / /home /var 等

**details:** nixos不建议将efi分区挂载到/boot/efi下面，直接创建一个512MB的分区给/boot即可。

> chroot

1) 创建configuration.nix配置文件使用`nixos-generate-config`指定位置
2) hardware-configuration.nix自动生成
3) 修改配置文件，声明新系统样子
4) 最小化配置可以在nixos网站找到
5) 安装新系统使用`nixos-install`
6) 为root用户设置密码
7) 创建非root用户即配置密码

**details:** 配置时尽量最小化，各种tools可以新系统安装完成后考虑。

> reboot

1) 安装完成
2) 配置网络等程序包
3) ...

至此，nixos的安装就完成了。这里为什么不详细记录每个步骤的完整过程呢？因为相关过程需要自己完善，逐渐形成我们自己的一套安装流程，安装系统就是跟着引导一步一步走，出现问题，自己解决，才能从中学到东西，很喜欢一句话：没有风暴的水面不是海洋，而是泥塘。
