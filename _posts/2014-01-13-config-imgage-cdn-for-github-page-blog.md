---
layout: post
title: 博客图片镜像CDN加速
categories: Jekyll
tags:
- jekyll
- cdn
- 七牛
---

 博客文章的配图以前用的是[imgur.com](http://imgur.com)一个国外的图床，国外服务器嘛，GFW的缘故访问时好时坏，最近看到七牛的镜像存储应该可以很好的实现jekyll的图片加速。[七牛镜像存储介绍](http://kb.qiniu.com/how-to-use-image-storage-and-qrsync)

####配置步骤

+ 注册[七牛](http://www.qiniu.com)账户,七牛新用户都有免费空间和流量配额

+ 进行**空间设置** ，设置**镜像源**
> 这个地方详细说下，这里的镜像源，是指放博客图片的文件夹，例如本站博客图片放在`freshtu.com/assets/img`下，则应将镜像源设置为`http://freshstu.com/assets/img/`即可，但是如果你的博客图片不跟文章存放在一起，那么我推荐直接用七牛空间做图床也可以,将图文放在一起的好处是便于搬家，分离好处是节省空间，毕竟github分配的空间是有限的

+ 配置jekyll的_config.yml文件 添加新行    
`IMG_PATH: http://freshstu.qiniudn.com`    即对应七牛空间的域名

####使用
以上配置完成后，以后写blog时，需要图片时可以将图片放到对应`assets\img`目录下，相应的markdown语法 `![Alt text](\{\{site.IMG_PATH}}/picture.jpg)` \(**记得去除转移字符**\) ,这样一来当访客第一次访问时，访问的还是原站的数据，但同时这一时期访问七牛CDN地址，七牛会将原站图片镜像到你创立的空间中,以后的访问就可以直接通过七牛访问，速度会明显加快

####测试：
![测试图片]({{site.IMG_PATH}}/35YOtX2.jpg)

上面的图片是通过cdn加速的，访问[这里](http://freshstu.com/2013/02/install-raspbmc-on-raspberry-pi/),第一张图是放在imgur.com的，比较一下吧，确实速度大有改观^_^

