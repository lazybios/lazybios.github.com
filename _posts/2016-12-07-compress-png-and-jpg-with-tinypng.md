---
layout: post
title: "在线无损压图工具TinyPNG"
date: "2016-12-07"
---

经常会在文章中放一些屏幕截图，这些图片一般都是用Mac自带截图工具截的，这样截出来的图片都会比较大。放到微信这样的移动平台上就会比较吃流量。尽管微信会做一些压缩，但依然不能改变图片较大的问题，所以最好的做法是在源头控制，在上传之前就做好压缩工作。

Mac上这类压缩软件有不少，不过大多数不能做到图片种类通吃，比如JPEGMini就只能压缩JPEG格式的图。鉴于此，今天来分享一个不错的在线压图工具TinyPNG。

### 使用

表面上TinyPNG看起来好像只能处理PNG，但实际上它还有个兄弟站叫TinyJPG，并且无论使用哪个入口站点，都可以同时处理这两种格式，使用方式也简单直接拖拽上传就可以了。不过美中不足的是TinyPNG限制每天上传图片的数量为20张，并且每张图大小不超过5M。虽然比不上那些离线压缩工具，但对于我这样的低频用户其实也足够了。

![tinypng-compress]({{site.IMG_PATH}}/tinypng-compress.png)

另一个值得一提的地方是，TinyPNG提供了比较完善的API服务。通过这些API，你可以把它无缝接入到你的APP或者工作流中，比如利用其提供的Node.js库结合Gulp压缩前端页面中遇到的图片，又或者借助API异步压缩用户产生的图片。

##### 压缩当前文件夹下的unoptimized.jpg
```sh
curl https://api.tinify.com/shrink \
     --user api:YOUR_API_KEY \
     --data-binary @unoptimized.jpg \
     --dump-header /dev/stdout
```

需要说明的是，对于免费用户TinyPNG只提供了每月500次的压图配额。不过我想一般用户应该不会超过这个量吧。

-完-

### 参考引用
+ [https://tinypng.com](https://tinypng.com)