---
title: "利用闲置 PC 搭建 NAS 媒体中心 远程下载"
date: 2017-12-03T10:00:00+08:00
draft: false
tags: ["nas", "plex", "aria2","trial"]
slug: ""
---

## 需求

1. 私有云存储
2. 家庭媒体中心
3. 远程下载

## 现有条件

网络：百兆移动宽带 （上行 3MB/s 左右）

路由器： 极路由 4 千兆网口

主机：闲置笔记本一台 （Windows 10 ；12G 内存 ；CPU i5 6300hq 2.3ghz ）

## 实现使用的解决方案和工具

seafile 实现云存储

aria2 远程下载

plex 媒体中心

## 远程访问

反向代理 frp （移动宽带分配的是内网 IP）

teamviewer （支持内网穿透）

## Windows 上的配置

开机自启

打开运行 输入下面的命令 会打开一个开机启动项的文件夹，把要开机启动的脚本放进去即可

    shell:Common Startup

希望脚本运行后不退出窗口，需要在脚步文件内添加如下的内容

    @echo off
    ...
    cmd

![](https://static.notion-static.com/d2ce2dd56df04271802bffb5e9af75da/Untitled)

## 访问速度优化

将家里的路由器 dns 中 将 以上几个服务的域名指向到内网的 ip，这样在家访问就走内网，访问速度大大提高。

实现上面的功能需要主机上配置 Nginx 将内网的服务端口转发到 80 端口并且指定对应的域名。

![](https://static.notion-static.com/96cd9be9f85c47448275804a60d36dfe/Untitled)

## 其他问题和建议

plex 编码占用大量 CPU

plex 可以实现 gup 编码，来降低 cpu 占用，收费 $5/月

aria2 百度云下载速度太慢

aria2 原版最大线程限制为 16，而百度云单线程只有 100k 左右，可以使用一个不限制线程的版本来破解。

[X86](https://ci.appveyor.com/api/projects/myfreeer/a780c730b7282e090f238e8286f815f3/artifacts/aria2c_x86.7z) [X64](https://ci.appveyor.com/api/projects/myfreeer/a780c730b7282e090f238e8286f815f3/artifacts/aria2c.7z)

nextclould

有 app 中心，但是很可惜被墙了

## 参考资料

[https://github.com/fatedier/frp](https://github.com/fatedier/frp)

[https://www.plex.tv](https://www.plex.tv/)

[https://www.seafile.com](https://www.seafile.com/home/)

[https://github.com/aria2/aria2](https://github.com/aria2/aria2)

[https://github.com/redapple0204/my-boring-python/blob/master/破解百度云限速.md](https://github.com/redapple0204/my-boring-python/blob/master/破解百度云限速.md#方法六采用aria2不限制线程版本)"
