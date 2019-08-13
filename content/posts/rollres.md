---
title: "写了一个 js css 在线联调工具 RollRes "
date: 2019-08-13T10:37:13+08:00
draft: false
tags: ["project", "RollRes"]
keywords: []
description: ""
slug: "rollres-change-the-response-of-the-request"
---

最近在做一个 [NHK 奥运会](https://sports.nhk.or.jp/olympic/)的项目,目前进入的开发的第二阶段，数据也越来越多。

我们知道，开发的测试环境和生产环境的数据可能会有很大的不同，测试环境无法 100% 模拟生产环境，有些时候我们需要处理一些前端的展示问题，但是测试环境无法模拟的情况下怎么办呢？不可能改一点就部署到生产环境去测吧，这属于撞大运编程，而且部署的时间成本也很高。

既然 debug 的成本都很高，有没有一种方法可以快速，低成本，高效的解决这个问题呢？

想必大家早已猜到，那就是将生产环境的资源文件（js，css）替换为本地开发的版本即可。

实现此类功能的扩展很多，例如 chrome 上就有 [ReRes](https://chrome.google.com/webstore/detail/reres/gieocpkbblidnocefjakldecahgeeica)

![](https://lh3.googleusercontent.com/va7QqVzWp6dZMAl-x4M9i9fF1gnAQPVbnZeEsCO_ZuE0r-K_zZ2sM1xz40TgK-l1AjeOfDSnhg=w640-h400-e365)

好用，就是界面有点看着难受，作者也很久没有维护了，我用了一段时间，实在受不了这个审美，也找不到其他类似的扩展，于是研究了它的核心代码，实现了一个 [RollRes](https://chrome.google.com/webstore/detail/rollres-change-the-respon/hbikkeelnonljdhjooeplbegdmfkjhbk) ，仅仅 18.92KiB 大小，远远小于同类扩展。

![](/img/rollres/1.png)
![](/img/rollres/2.png)
![](/img/rollres/3.png)

大家可以在 [chrome 应用商店](https://chrome.google.com/webstore/detail/rollres-change-the-respon/hbikkeelnonljdhjooeplbegdmfkjhbk) 找到该扩展，如果觉得这个扩展对你有用，请点赞，评论哦。
