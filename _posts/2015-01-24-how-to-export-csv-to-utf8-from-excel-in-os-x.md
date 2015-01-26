---
layout: post
title: Mac系统下Excel导出UTF8格式的CSV
categories:
- mac pro
tags:
- os x
---

想必使用mac的开发人员，一定遇到处理过来自win平台下的excel情况，当然用win excel打开是没有一点问题的，但是如果你需要利用csv格式写一些自动化的东西，那一定会遇到恶心的编码格式问题。

下面说说我遇到的情况，我拿到的excel格式是`latin1`和`gb2312`,后面那个还比较好搞定，通过vim下`set fileencoding=utf-8`就轻松搞定了，但是前者用这个却行不通,折磨了半天，最后想到个变通的办法，那就是用`Numbers`打开,然后做csv导出,具体`File->Export->CSV`,然后在高级设置里选择要转换的编码格式，备选编码还是比较丰富的

![Numbers export to csv]({{site.IMG_PATH}}/numbers.png)



-完-
