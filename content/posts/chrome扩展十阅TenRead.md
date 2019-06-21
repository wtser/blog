---
title: "chrome 扩展 十阅 （TenRead）"
date: 2015-04-20T10:01:16+08:00
draft: false
tags: ["chrome extension", "rss", "project"]
keywords: []
description: ""
slug: ""
---

## 为何要做这个东西

### 满足自身需求

之前一直有订阅 rss 的习惯，有博客也有各种聚合站。

博客类的站点更新频率不是特别频繁，但是几个聚合站点，例如 hacker news 等，每日更新内容特别多，每天都看不完。日积月累，一个月下来就要奔溃了，而且很多内容是及时性比较强，到月底再去看已经过时了。

之前发现了一个软件叫 一览，能浏览几个热门网站的精华文章，随时想看就能看到最新的热门文章。于是我把 rss 里面的部分订阅取消，然后用 一览 来解决。

用了一段时间发现，一览体验虽好，但是提供的网站就那么几个，而且不支持添加自定义站点，造成了很大的限制。

为了满足自己的这个需求，我决定开发类似功能的 chrome 扩展程序，十阅 因此诞生了。

### 锻炼开发技能

最早还不会 angular，使用的是 backbone jQuery 开发，开发这个扩展用了 2 天时间，实现了阅读新闻和管理阅读源的功能，就上谷歌商店上了第一版本。

第二版在第一版的基础上，增加了订阅商店的功能。降低添加阅读站点的难度。

目前的第三部，使用了 angular 进行了重构，对界面和交互也进行了微调。使用 gulp 功能把前端构建环境也搞起来，使用 bower 管理第三方依赖。还使用了 sass。接下来可能还会搞搞 coffee script。

### 贡献社会（开源）

这个项目托管于 [github](https://github.com/wtser/simpleNews--ext) 和 [coding.net](https://coding.net/u/wtser/p/ten-read)

核心代码其实很简单，有兴趣的朋友可以 fork 一份，有问题也可以发个 issue 给我。

## 有了 RSS 为何还需要它

这个扩展解决了以下几个问题

### 解决信息过载

让你从信息爆炸的灾难中解脱出来。

### 实时获取热门文章

只关注热点文章，提高阅读效率

### 利用碎片化时间

## 愿景

- 让更多的人受益于此项目
- 希望更多人加入开源行列

## 功能实现使用的技术与工具

- angular
- zepto
- sass
- gulp

## 功能介绍

扩展默认预置了一些网站。你可以在配置页面进行 增删改。

扩展配置页面提供了订阅商店的功能，目前已经添加了若干站点，不断完善中。

## 资源链接

[下载 chrome 扩展](https://chrome.google.com/webstore/detail/%E5%8D%81%E9%98%85/bpnpkcfhagdgccpikdkldbnngifepibc)

## 界面预览图

![popup][1]

![配置页面][2]

![配置页面][3]

[1]: https://image-static.segmentfault.com/312/917/3129176156-5535160977568_articlex
[2]: https://image-static.segmentfault.com/102/079/1020794188-553515ffddfc7_articlex
[3]: https://image-static.segmentfault.com/106/578/1065781510-553515ec64def_articlex
