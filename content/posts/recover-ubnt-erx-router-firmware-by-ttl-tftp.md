---
title: "使用 TTL 和 TFTP 恢复 ubnt erx 官方固件"
date: 2019-11-06T10:37:13+08:00
draft: false
tags: ["trial", "ttl", "tftp", "ubnt", "firmware","erx"]
keywords: []
description: ""
slug: "recover-ubnt-erx-router-firmware-by-ttl-tftp"
---


## 为什么要恢复

因为我把它刷成了 openwrt ⇒ 为了安装 openclash ⇒ 为了更好的展开工作

但是感觉不好用，并且打算通过旁路由的形式实现。

## 怎么用 TTL

1. 购买一个 USB to TTL 设备，然后把相关的针脚连接好
2. 将USB插入电脑，识别后连接到对应的端口等待输出
3. 给路由器通电，正常情况下可以看到启动的输出信息
4. 按需选择启动模式，进行后续操作

## ERX 教程

1. 连接 TTL

    TX RX GND 线都接好接正确，不正确会导致不输出或者乱码（我遇到了没接地线乱码）。

    使用 putty 填入对应的 COM 端口（我这里识别的是 COM3）和速率 57600，然后点击 open 进行连接，速率一定要设置正确，否则输出乱码。

    ![](/img/Untitled-cc4409d4-aa9f-482f-8844-50502e809e2b.png)

    给路由器通电，这时候 putty 窗口有内容输出则说明前面的步骤均正确，可以进入下一步操作了。

2. 使用 tftp 恢复
    1. 下载恢复镜像 （此步骤可提前）
        1. [https://dl.ui.com/firmwares/edgemax/v2.0.x/ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img](https://dl.ui.com/firmwares/edgemax/v2.0.x/ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img)
        2. [https://dl.ui.com/firmwares/edgemax/v1.10.x/ER-e50.recovery.v1.10.10.5210345.190714.1127.16de5fdde.img](https://dl.ui.com/firmwares/edgemax/v1.10.x/ER-e50.recovery.v1.10.10.5210345.190714.1127.16de5fdde.img)
    2. 连接网线

        ![](/img/Untitled-fed04cb9-8746-4e6c-9044-7f79535dc0eb.png)

        上图只是一个示意，将电脑的lan口和路由器的 eth0 口连接，并假设路由器 IP （device IP） 是 192.168.1.20，电脑 IP（server IP） 为 192.168.1.10，其中电脑 IP 需要自行配置好。

    3. 设置好tftp服务

        [http://tftpd32.jounin.net/tftpd32_download.html](http://tftpd32.jounin.net/tftpd32_download.html)

        打开后设置好恢复镜像所在的目录和服务器地址。

        ![](/img/Untitled-59d62974-22a3-439d-84d6-dd38130051ef.png)

    4. 恢复固件

        在路由器启动的过程中，按数字1键，可以中断启动进程，选择启动模式，如下所示。

            Please choose the operation: 
             1: Load system code to SDRAM via TFTP. 
             2: Load system code then write to Flash via TFTP. 
             3: Boot system code via Flash (default).
             4: Entr boot command line interface.
             7: Load Boot Loader code then write to Flash via Serial. 
             9: Load Boot Loader code then write to Flash via TFTP. 
             default: 3

        输入1，然后填入对应的 IP和恢复镜像的名称。

            1: System Load Linux to SDRAM via TFTP. 
            Please Input new ones /or Ctrl-C to discard
            Input device IP (172.16.3.211) ==: 192.168.1.20
            Input server IP (172.16.3.210) ==: 192.168.1.10
            Input Linux Kernel filename (vme600) ==: ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img

        没有报错的情况下（例如确保网线是连通的），耐心等待恢复完成即可。

## 案发现场

![](/img/Untitled-17befbef-9405-47de-baad-84704f85f0a3.jpg)

![](/img/Untitled-42f29356-c37a-4ee6-a654-0a909a81ec36.jpg)

![](/img/Untitled-17d2ace9-7b3f-41ad-a725-477d4cdb4142.jpg)

![](/img/Untitled-2b93cd99-4830-4f59-b779-e34e74229bfa.jpg)

![](/img/Untitled-20ef7503-58ad-461e-be94-2d8431ff733b.jpg)

## 参考资料

[https://help.ubnt.com/hc/en-us/articles/360018189493-EdgeRouter-Manual-TFTP-Recovery#1](https://help.ubnt.com/hc/en-us/articles/360018189493-EdgeRouter-Manual-TFTP-Recovery#1)

[https://an.undulating.space/post/181228-erx_install_openwrt_recover_edgeos/](https://an.undulating.space/post/181228-erx_install_openwrt_recover_edgeos/)

[https://www.right.com.cn/forum/thread-194070-1-1.html](https://www.right.com.cn/forum/thread-194070-1-1.html)