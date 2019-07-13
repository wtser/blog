---
title: "重构 qrCreater 网址二维码一键生成工具"
date: 2019-07-13T10:37:13+08:00
draft: false
tags: ["project", "QR code"]
keywords: []
description: ""
slug: "reconsitution-of-qr-creater-extension"
---

最近天天加班，无奈。频频要到手机上去测试页面。最快捷的方法就是把网址生成为一个二维码，手机上一扫即可访问。

自己写过一个 chrome 扩展，上一次的版本还停留在 2014 年，当时的我刚刚毕业不久，用的 jquery qr 插件实现。

界面简陋，功能好用，过去了这么多年，现在再来看当年做的，感觉太丑了，而且在 dark mode 下 icon 也看不清了，索性就来改一改。

## 第一版

icon 改成了暗红色，去掉了底部的输入框生成二维码，因为我自己极少用到这个功能。

![qrCreater 1.2](https://camo.githubusercontent.com/bc552b38f56d4cd19053413f14ab96ab8724c576/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f32305f4947504346386d73305a66315a6f5247564771666e3655386674396376326e7554504f5334715573624d6573366155695a626e64393776537863635371626154357852432d5f506b3d773634302d683430302d65333635)

周末发布，周一就有老同事找我，说你的插件怎么把输入框功能去掉了。

感动，想不到这么多年，老同事一直在用我写的插件，想着抽空完善一下。

## 第二版

今天抽空再次完善了一下，加入了输入框，兼顾美观。

替换了臃肿的 jquery qr，采用了更加小巧的 kjua，最终扩展的体积缩小到了 14.47KiB。

顺便说一句，用 parcel 来打包真的很方便。

![qrCreater 1.3](https://lh3.googleusercontent.com/w3z8kDO1PlYpZaYRvj94k9wMrBLdt8z5fyzM-2uVrYaxZumOMzbPy_8jqKzL7ezR0HA9VO209Q=w640-h400-e365)

大家可以在 [chrome 应用商店](https://chrome.google.com/webstore/detail/qrcreater-%E7%BD%91%E5%9D%80%E4%BA%8C%E7%BB%B4%E7%A0%81%E4%B8%80%E9%94%AE%E7%94%9F%E6%88%90%E5%B7%A5%E5%85%B7/kpbkbekmhiifnijhhfmahckmlbcbinfc) 找到该扩展，如果觉得这个扩展对你有用，请点赞哦。
