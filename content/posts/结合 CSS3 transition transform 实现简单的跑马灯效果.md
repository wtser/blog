---
title: "结合 CSS3 transition transform 实现简单的跑马灯效果"
date: 2018-02-06T10:00:00+08:00
draft: false
tags: ["css3", "transition", "transform", "project"]
slug: ""
---

这是之前客户的一个需求，给的 demo 是用 gif 实现跑马灯，但是我们的没法用 gif，因为图上的文字需要翻译成各国语言，所以不能使用图片来实现，那么，自己写一个咯。

## 思考过程

![思考过程](https://image-static.segmentfault.com/229/355/2293555127-5a7d1130d6296_articlex)

## html

```
<div lantern>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
  </ul>
</div>
```

## css

```css
* {
  margin: 0;
  padding: 0;
}

[lantern] {
  overflow: hidden;
}

ul {
  white-space: nowrap;
  font-size: 0;
  transform: translateX(0);
  transition: transform 0s linear;
}

li {
  width: 50vw;
  border: 1px solid red;
  display: inline-block;
  height: 30px;
  font-size: 16px;
}
```

## js

```js
function lantern($element, speed = 10) {
  let liWidth = 0;
  let $ul = $element.find("ul");

  function run(init = false) {
    let $li = $ul.find("li").first();
    liWidth = $li.outerWidth();

    if (!init) {
      $ul.append($li[0].outerHTML);
      $li.remove();
    }

    $ul[0].style.transitionDuration = "0s";
    $ul[0].style.transform = "translateX(0)";

    setTimeout(function() {
      $ul[0].style.transitionDuration = speed + "s";
      $ul[0].style.transform = "translateX(-" + liWidth + "px)";
    }, 20);
  }

  run(true);
  setTimeout(() => {
    setInterval(run, speed * 1000);
  }, 0);
}

lantern($("[lantern]"), 20);
```
<iframe height="265" style="width: 100%;" scrolling="no" title="结合 CSS3 transition transform 实现简单的跑马灯效果" src="//codepen.io/wtser/embed/GyNzBK/?height=265&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/wtser/pen/GyNzBK/'>结合 CSS3 transition transform 实现简单的跑马灯效果</a> by 王铁手
  (<a href='https://codepen.io/wtser'>@wtser</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>