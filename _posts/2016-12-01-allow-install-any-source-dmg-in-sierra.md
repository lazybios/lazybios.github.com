---
layout: post
title: "macOS Sierra中允许安装任意来源应用"
date: "2016-12-01"
---

默认情况下Mac只允许安装来自Mac App Store的应用，macOS Capitan 时期我们可以在「安全与隐私」中更改这一配置:

![seirra-setting]({{site.IMG_PATH}}/seirra-setting.png)

但是从macOS Sierra 10.12开始，「安全与隐私」设置中隐藏了「任何来源」这一选项，即不允许我们安装来自身份不明开发者提供的应用了。

这样的设置固然能起到安全防护的作用，但也一起把那些安全但没有签名的应用拒在了门外，那么有没有一个办法能回到从前呢？还真有，方法如下:

##### 终端(Terminal)中执行下面指令

```
sudo spctl --master-disable
```

重新打开「系统偏好设置」-> 「安全与隐私」就又能重新看到「任意来源」这一选项了。


-完-