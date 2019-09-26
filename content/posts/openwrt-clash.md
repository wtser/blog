---
title: "Ubnt erx 刷 openwrt 并用上 clash"
date: 2019-08-13T10:37:13+08:00
draft: false
tags: ["trial", "ubnt", "openwrt", "clash"]
keywords: []
description: ""
slug: "install-openwrt-on-erx-and-using-clash"
---

## 起因

erx  原厂的固件，安装[别人写好的 SS 脚本](https://github.com/izerosoul/shadowsocks_erx)，基本满足需求，但是 dns 解析偶尔抽风，我还在作者项目页面[提了issue](https://github.com/izerosoul/shadowsocks_erx/issues/37) 。

作者很久没维护了，想了想只能靠自己了。搜了一圈发现 [clash 有openwrt 版本](https://github.com/frainzy1477/clash)，试着部署到 erx 上玩玩。

## 过程

1. 刷 openwrt
2. 安装 clash
3. 安装 [luci-app-clash](https://github.com/frainzy1477/luci-app-clash)

    解决依赖

## 结论

一路折腾下来，基本上还是顺的。主要遇到 2 处问题。

一个是刷 openwrt 的时候，按照官方提供的指南，使用其提供的最新版镜像，添加系统镜像不成功，后来我自己找了一个别人发的早期教程，下载他提供的低版本就能用。

还有就是安装 luci-app-clash 的时候，要解决依赖问题，以及解决 openwrt 自带 dnsmasq 与其依赖 dnsmasq-full 冲突 （卸载 dnsmasq 然后安装 dnsmasq-full 就行了）。

## 配图

![openwrt 主界面](/img/Untitled-31e89b2c-cde5-4f8a-aed1-5eded5a7eeb0.png)

openwrt 主界面

![clash 界面](/img/Untitled-fb16a77b-29b6-4113-a0f9-c0f9223bf7d7.png)

clash 界面

## 参考链接

- openwrt 相关

    [https://openwrt.org/toh/ubiquiti/ubiquiti_edgerouter_x_er-x_ka](https://openwrt.org/toh/ubiquiti/ubiquiti_edgerouter_x_er-x_ka)

    [https://an.undulating.space/post/181228-erx_install_openwrt_recover_edgeos/](https://an.undulating.space/post/181228-erx_install_openwrt_recover_edgeos/)

- 代理相关

    [https://github.com/wtser/shadowsocks_erx](https://github.com/wtser/shadowsocks_erx)

    [https://github.com/izerosoul/shadowsocks_erx](https://github.com/izerosoul/shadowsocks_erx)

    [https://github.com/frainzy1477/clash](https://github.com/frainzy1477/clash)

    [https://github.com/frainzy1477/luci-app-clash](https://github.com/frainzy1477/luci-app-clash)

- 抗 DNS 污染

    [https://github.com/Fndroid/clash_for_windows_pkg/wiki/DNS污染对Clash（for-Windows）的影响](https://github.com/Fndroid/clash_for_windows_pkg/wiki/DNS%E6%B1%A1%E6%9F%93%E5%AF%B9Clash%EF%BC%88for-Windows%EF%BC%89%E7%9A%84%E5%BD%B1%E5%93%8D)

    [https://github.com/frainzy1477/luci-app-clash/wiki/Fake-IP-Mode](https://github.com/frainzy1477/luci-app-clash/wiki/Fake-IP-Mode)

    [https://github.com/frainzy1477/clash/issues/31](https://github.com/frainzy1477/clash/issues/31)

    [https://github.com/shadowsocks/ChinaDNS](https://github.com/shadowsocks/ChinaDNS)

    [https://cyberloginit.com/2019/04/08/chinadns-code-analysis.html](https://cyberloginit.com/2019/04/08/chinadns-code-analysis.html)

- 本案所用的文件

    链接: [https://pan.baidu.com/s/1nElWLeEVD1NM7c1n3_LUVA](https://pan.baidu.com/s/1nElWLeEVD1NM7c1n3_LUVA) 提取码: 74jf