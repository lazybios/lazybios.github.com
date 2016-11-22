---
layout: post
title: "搭载Android 7.0的Nexus 5X Root指南"
date: "2016-11-22"
---

因为担心Root以后无法正常OTA升级，所以自入手Nexus 5X以来一直这么跑着，完全是人肉拿耐心跟国内的流氓APP做抗争。刚升级7.0那会儿还好，不过最近的速度完全是没法忍了，特别是在跟朋友出去吃饭要结账的时候，那个卡呀，搞的跟不想掏钱似的，太被动了(0.o)。

所以Root是必须的了，Android 7.0推出到现在，国内ROM厂商还没几家这么快跟进上来的，本来想着能用个啥第三方辅助工具给一键Root了，找了一圈没找到，还是得手动搞。先来交代一下工具与环境吧。

+ **PC**: Mac OS X (其实操作系统是啥一点也不重要，互相都有替代，只要操作顺序和刷的内容一样就行了)
+ **手机**: Nexus 5X，系统Android 7.0

在多提醒一下，Root以前一定记得要备份好原手机中的数据，另外记得要保持电量充足哈。

### 安装adb与fastboot
在Mac上安装`adb`与`fastboot`，直接用Homebrew就可以了，简单快速，不用来回折腾文件的位置。

```
brew install android-platform-tools
```

没安装过`brew`了自行检索安装吧。安装好后，可以尝试在终端中敲`adb`与`fastboot`看有没有反应，有输出表示安装成功。


### 解锁bootloader
装好`adb`与`fastboot`后，这一步来解锁bootloader，在正式解锁之前，请一定要确保已经备份好数据了，因为解锁的过程会格式化系统。

首先在「开发者选项」中将「USB调试模式」与「OEM锁」分别置于打开状态，之后就可以在PC终端中通过`adb`让手机进入到bootloader界面。

```
adb reboot bootloader
```

![bootloader-before]({{site.IMG_PATH}}/bootloader-before.jpg)

进入上图画面后，在执行下面语句解锁OEM

```
fastboot oem unlock
```

成功之后会重启再次回到bootloader界面，不过这次可以在画面中看到`unlock`的字样，见下图:

![bootloader-after]({{site.IMG_PATH}}/bootloader-after.jpg)

到这里bootloader就算顺利解锁成功，可以通过音量键选择`start`状态重启系统，这时手机就跟当初刚入手时候一样啥都没有，不过不要紧静静的等待刷入Recovery吧。


### 刷入第三方Recovery系统TWRP
这一步我们需要预先准备两个文件，分别是SuperSU与TWRP：

+ [SuperSU](http://t.cn/RfSFJx7) 权限管理工具
+ [TWRP （TeamWin Recovery Project）](https://twrp.me/devices) 辅助刷机的Recovery系统

建议大家都到它们各自官网上下载最新版本，不建议直接从别人的网盘拿，有些文件存在网盘里早就过时了，刷错版本了搞不好要变砖的。


##### Supersu下载页
![supersu]({{site.IMG_PATH}}/supersu.png)

##### Twrp下载页
![twrp]({{site.IMG_PATH}}/twrp.png)

在Devices根据自己型号，解锁对应的版本，点进去下最新的。准备好上面两个文件后，我们需要把它们分别放置到合适的位置:

+ SuperSU下载好后将它放到手机的根目录下即可，只要能在后面的刷机中通过目录方便定位到就行。
+ TWRP镜像下载完以后，直接丢桌面就好了

文件与位置就位后，我们需要再次进入到Bootloader画面，使用`fastboot`来刷入Twrp

用数据线让PC与手机保持连接状态，关机后，先按「向下音量键」，再摁「关机键」同时摁住保持不动若干秒就后就进入到Bootloader。

进入Bootloader后，在PC终端执行下面`fastboot`指令刷入刚才下载好的twrip镜像，需要说明的是要确保后面twrp镜像位置是对的，刚才我们直接把下载好的文件丢到桌面了，所以这时你应该在桌面目录下执行才对。

```
cd ~/Desktop
fastboot flash recovery twrp-3.0.2-2-bullhead.img
```

这一步特别快基本不用等，看到下图画面就表示Recovery刷入成功。

![fastboot-twrp]({{site.IMG_PATH}}/fastboot-twrp.png)

再次回到bootloader界面，上下按动音量键，切换进入到Recovery Mode，也就是Twrp的界面，点击进入Install选项，这里它会让你选择要安装的文件，就是前面我们准备好的SuperSu文件，找到确认以后完成安装以后重启就算是root成功了。

![TWRP-flash-SuperSU]({{site.IMG_PATH}}/TWRP-flash-SuperSU.jpg)

-完-

### 参考引用
+ [http://t.cn/RfSgGSq](http://t.cn/RfSgGSq)
