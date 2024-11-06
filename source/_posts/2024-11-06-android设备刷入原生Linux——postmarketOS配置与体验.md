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
