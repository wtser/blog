---
title: "极路由 4 MAC 变 0 恢复"
date: 2020-03-01T12:46:13+08:00
draft: false
tags: ["trial", "firmware","hiwifi"]
keywords: []
description: ""
slug: "recover-gee4-mac"
---


## 奇怪的现象

不知道什么时候起， 总会分配一堆 ipv6 地址，让人诧异抓狂。

![many ipv6 address](/img/recover-gee4-mac-1.png)

后来发现路由器的 wifi BSSID总是会变，估计与此有关。刷回官方估计，发现 mac 地址后6位变成0了，典型的半砖状态。

## 失败经验总结

1. 在 breed 下 刷 eeprom 无效
2. 在 breed 下 改 MAC 地址 无效

## 一次验证性的尝试

在 breed 下完整克隆了另一台路由的编程器固件，然后恢复，发现 MAC 地址变了，两台路由器MAC一模一样，然后开始干扰和冲突了。很好，至少说明 MAC 地址能改。

## 靠谱解决方案

经过研究，发现只要有 bdinfo.bin oem.bin，这两个文件就可以解决 MAC 地址问题

    mtd write bdinfo.bin bdinfo
    mtd write oem.bin oem

SSH 下 刷入 MAC 就恢复了，因为我自己的没有备份，找的别人分享的，所以最终 MAC 地址也是别人的，如果有人知道如何改 MAC 地址，请告诉我。

MAC 地址稳定后 IPV6 分配也正常了。

![only 2 ipv6 address](/img/recover-gee4-mac-2.png)

参考资料

[https://www.right.com.cn/forum/thread-320455-1-1.html](https://www.right.com.cn/forum/thread-320455-1-1.html)

[https://www.right.com.cn/forum/thread-269196-1-1.html](https://www.right.com.cn/forum/thread-269196-1-1.html)

[https://www.right.com.cn/FORUM/thread-304491-1-1.html](https://www.right.com.cn/FORUM/thread-304491-1-1.html)