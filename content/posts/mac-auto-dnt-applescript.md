---
title: "Mac 勿扰模式周期性开关闭功能实现脚本"
date: 2017-10-30T10:33:00+08:00
draft: false
tags: ["applescript", "DNT"]
slug: "auto-dnt-for-mac-with-applescript"
---
![](https://sfault-image.b0.upaiyun.com/992/660/992660630-59f6de4fba1ae)

专心开发工作的时候，通知滴滴答答响个不停，影响效率和心情? 。

发现 Mac 提供了临时屏蔽通知的功能，就是 勿扰模式。
不错，可以安心工作了。缺点就是容易忘记关闭勿扰，错过一些重要的通知。
可是每次手动开启关闭又显得麻烦。如果我正在使用番茄钟工作，想要25分钟勿扰 5分钟尽管扰呢？

AppleScript！这货系统自带的，好用，几行脚本就轻松实现了。


前提：在系统快捷键中 将打开关闭勿扰模式的快捷键设置为 cmd+shift+option+control+d

### applescript
```applescript
repeat while true
	ignoring application responses
		tell application "System Events" to keystroke "D" using {command down, shift down, option down, control down}
	end ignoring
	say "DND opened"
	delay 1500
	
	ignoring application responses
		tell application "System Events" to keystroke "D" using {command down, shift down, option down, control down}
	end ignoring
	say "DND closed"
	delay 300
end repeat
```
