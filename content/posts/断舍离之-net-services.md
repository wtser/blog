---
title: "断舍离之 net services"
date: 2019-05-25T13:22:12+08:00
draft: false
tags: [断舍离, vultr, bandwagonhost, CN2, GIA,"trial"]
keywords: []
description: ""
slug: ""
---

使用 vultr 也有相当长的一段时间了，最近苦恼于其速度不太理想，晚上高峰时间更是接近不可用的状态，不得不思考新的出路。

调查研究后明白只能更换线路才能彻底解决，最好的线路是 CN2 GIA。

购买了 bandwagonhost CN2 GIA 线路的 vps，更换线路后，速度稳定很多，但是 vps 配置较差，内存只有 512M，很多应用编译都困难，更不要说运行了。

考虑到 vultr 虽然配置高，但是部署的服务大部分时间属于闲置吃灰状态，决定不用 vultr 以节省部分开销。

## 自己目前的需求

1. proxy
   1. 出国加速
   2. 反向代理
2. blog
3. cloud storage

## 思考了一下，最终的解决方案
    
  - **proxy 用 bandwagonhost cn2 gia 线路 vps**
  ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Feeeb66e8-32c1-4f41-8d70-a0a13ccb21d8%2FUntitled.png?table=block&id=ffa4a69a-781b-4b0d-8595-d0dfb18673e9&width=2530&cache=v2)

  - **blog 使用 github pages 和 coding pages 做双线部署，国内国外分开进行访问加速。**
  ![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe660419d-a993-4b6d-9cd2-67df82ceba1f%2FUntitled.png?table=block&id=c7aae48e-ed3e-41ad-a42a-78acf21ede67&width=1670&cache=v2)

  - **cloud storage 之前用的自建 seafile，迁移到 onedrive。**

## 参考文档
 - [国内三大运营商宽带线路及分级介绍](https://xn--9wy095f.com/blog/index.php/2018/05/01/142/)