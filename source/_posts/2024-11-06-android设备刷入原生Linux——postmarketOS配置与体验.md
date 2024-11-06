---
title: android设备刷入原生Linux——postmarketOS配置与体验
author: DingWH03
hide: false
archive: false
math: false
date: 2024-11-06 16:36:03
tags: [刷机, android, Linux]
categories:
- 简单记录
- android刷机
---
## 一、查找设备Linux支持情况

通常较旧的设备例如骁龙625/骁龙820/骁龙410等设备或是使用mtk处理器的chromebook有较好的PostmarketOS官方支持，可以通过查询[postmarketOS Wiki-Devices](https://wiki.postmarketos.org/wiki/Devices)网站查看是否有自己的设备支持。此外，被官方支持的设备(例如红米2和红米note4x)大多数有第三方开发者移植自己的Ubuntu或是debian，更多预编译Linux内核与Rootfs可以在互联网上搜索，当然有想法折腾的也可以自己克隆源代码编译。

需要注意的是，大多数设备都不能完美的支持Linux，即刷入Linux系统后手机的部分甚至是大部分设备例如摄像头和扬声器甚至GPU都是无法工作的，仅仅可以完成特定工作，或者只是为了好玩，否则Linux绝对没有android系统更适合你的设备。

> ***为什么Andorid设备可以启动Linux？甚至是Windows？***
>
> 通常，Android设备的引导程序是基于U-Boot或Bootloader等其他专有的引导加载程序，这与我们熟知的计算机有所不同。为了让Linux系统能够顺利启动，首先需要通过修改引导过程，使用像lk2nd这样的工具进行引导。lk2nd是一个轻量级的引导加载程序，bootloader引导lk2nd，lk2nd再去引导完整的linux内核，骁龙845设备利用UEFI启动Windows也是类似原理，但不是利用lk2nd。

这里我使用的android设备是红米note4x，其采用骁龙625处理器，性能不高但好在功耗够低，并且二手机价格足够便宜，非常适合折腾刷机，可以看到官方对该型号的支持较好，网上还有许多大佬制作的其他Linux发行版，但是硬件驱动确实PostmarketOS做的最好，甚至电话短信都是正常的。

{% asset_img supportinfo.png mido设备支持情况 %}

## 二、刷入

### 1. 下载或构建rom

对于PostmarketOS官方支持的设备，可前往[official images](https://images.postmarketos.org/bpo/)下载预构建Rom，非官方支持的型号可使用pmbootstrap软件自行构建，这里暂不讨论。
在[official images](https://images.postmarketos.org/bpo/)中，edge为滚动发行版本，软件为最新版。

其中`phosh`图形界面与`kde mobile`使用体验较好，可以自行选择体验。

### 2. fastboot刷机

下载下来的镜像通常会有3个，分别对应`lk2nd`、`boot`与`rootfs`，解压出img镜像后进行操作。

{% asset_img screenshot-2024-11-06-14-21-29.png downloadrom %}

首先将设备进入fastboot模式，刷入`lk2nd`

```bash
fastboot flash boot xxx-lk2nd.img
fastboot reboot
```

等重启后会进入lk2nd界面，在此界面会显示设备相关信息，继续使用fastboot命令分别刷入boot和rootfs到boot分区和userdata分区。

{% asset_img AGC_20241106_222452204.jpg lk2nd %}

```bash
fastboot flash boot xxx-boot.img
fastboot flash userdata xxx.img
fastboot reboot
```

再次重启后，即可引导开机。

{% asset_img AGC_20241106_222544907.jpg photo %}

## 三、启动

{% asset_img AGC_20241106_222616804.jpg photo %}

由于我选择内置phosh图形界面，启动成功会进入phosh的锁屏界面，默认账户密码为147147，输入成功即可解锁，第一次开机会出现欢迎界面。使用图形界面正常的连接wifi后，记下IP地址，还需要打开`Settings->System->Secure Shell`才可以愉快的使用ssh连接。

{% asset_img AGC_20241106_223924789.jpg photo %}

不过可能是bug，我尝试在图形界面中启用ssh并不生效，只能在终端中手动使用命令启用了。

```bash
sudo rc-update add sshd
sudo rc-service sshd start
```

> 使用sudo执行命令时，发现sudo自动被替换成了`doas`，据说会比sudo更安全。

执行完成后，本以为可以远程ssh访问了，事情似乎没那么简单，出现了下面的错误，在我尝试几次仍然无法连接之后，又重试几次，登陆成功了。非常奇怪。

```bash
dwh@pop-os ~ [255]> ssh user@10.0.0.142
The authenticity of host '10.0.0.142 (10.0.0.142)' can't be established.
ED25519 key fingerprint is SHA256:XeK2HjAjkr0hMIEalqMnb8a92Duve9Os805MRhp4r3g.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.0.142' (ED25519) to the list of known hosts.
(user@10.0.0.142) Password:
Connection closed by 10.0.0.142 port 22
```

## 四、配置系统

### 1. 更换软件源

上手一个全新的Linux第一步当然是更换软件源加速软件包下载，我直接使用ustc和tuna镜像源，直接将下面的配置内容放入`/etc/apk/repositories`即可。当然也可以使用其他镜像源。

postmarketOS默认是不安装`vim`的，但是支持`vi`。

```text
https://mirrors.tuna.tsinghua.edu.cn/postmarketOS/master
http://mirrors.ustc.edu.cn/alpine/edge/main
http://mirrors.ustc.edu.cn/alpine/edge/community
http://mirrors.ustc.edu.cn/alpine/edge/testing
```
