---
layout: post
title: raspberry_pi折腾记之raspbmc初体验
categories:
- Raspberry_pi
tags:
- raspbmc
- Raspberry_pi
- Linux
---

Raspberry_pi到手已经快半个月了，但一直因为考试的缘故没有好好研究。今天终于被我拿出来继续折腾一番，据说pi的1080p视频支持不是盖的，所以首先实现一下XBMC媒体中心,不知道XBMC的同学给个[梯子](http://baike.baidu.com/view/2132148.htm)

这是树莓派首次启动图，留个纪念
![Imgur](http://i.imgur.com/35YOtX2.jpg)
![Imgur](http://i.imgur.com/pJCgKpp.jpg)
####配置清单
+ rasp pi一枚
+ HDMI或VGA(含转接头)
> 这里特别不推荐购买上图中的HDMI转VGA接头，最好还是找一个带长线的吧，否则会带来诸多不便，因为大多数显示器视频接口都凹在塑料外壳中的且比较窄，这种轻便型的根本没办法插进去，即使是接口符合要求，所以如果你运气不好的话，周围显示器都是那样的，这个接口够不着显示器的接口，那么你的这个转接头也只能用来给pi用了-,-
+ 显示器
+ SD卡
> 最好是4g以上class 10的，因为是作为pi的硬盘来使用，免不了要有很多IO操作，我用的**SanDisk 闪迪 microSDHC Class10 8GB**感觉还不错
+ 网线或无线网卡
> 二者任意即可，但使用无线网卡的同学要注意，因为rasp_pi的额定电压是5V，这样如果在你的现有USB鼠标键盘之上再增加无线网卡，可能会导致供电不足，进而会出现无法启动，死屏之类的情况。我用的**FAST 迅捷 FW150US**,经管它已经很小了，但还是有供电问题，貌似玩这个东西，必须要有个有源的usb hub
+ 电源和micro数据线
> 电源随便用手机充电器就行，5V 1A的参数，很容易找到的，我用的就是华为手机上的充电器
+ 键盘、鼠标、音箱

####下载清单
+ xbmc系统
> 有两个分别是[xbian](http://xbian.org/)、[raspbmc](http://www.raspbmc.com)，本次折腾是后者raspbmc  [下载地址](http://www.raspbmc.com/download/)
+ xbmc-addons-chinese 中文扩展插件
> 插件名称：repository.googlecode.xbmc-addons-chinese-eden.zip 安装完成都是些国外插件，这个插件可以导入国内一些常用的视频网站插件，例如土豆、优酷之类的，[下载地址](http://code.google.com/p/xbmc-addons-chinese/downloads/list) **注意下载需要翻墙**   
还有一点要说明烧录SD卡成功后把下载的该插件放到/home目录中去，为设置raspbmc做准备

####安装步骤
#####1. 烧录raspbmc到SD卡
> 这一步很简单直接按照官方的start指南就可以了，官方有两种安装方式分别是windows、linux&mac，windows下的自不被多说，直接按照官方下载烧录工具并勾选协议，其它保持默认状态点Install就可以了，linux下安装需要下载安装脚本，修改为执行权限后，再执行此脚本，**下载脚本要下到SD卡内，同时执行也在这个目录里，事后再删掉安装脚本**，最后按照提示选择即可，特别提醒不要选错挂载目录，ubuntu下回会示显示整个电脑硬盘挂载和你的sd卡目录   
**建议无论是windows下还是linux下都把其它的移动存储设备拔掉，以灭搞错把文件给覆盖了**，还有一点烧录好的SD卡是linux文件格式的，所以在windows下读不出其它分区，只能看到引导分区
#####2. 给pi加电启动，设置raspbmc
> 加电后，raspbmc就开始自动启动了，因为是第一次使用，所以会花费不少时间，不过值得庆幸的是整个过程都是自动进行的，所以其实你就可以找些其它事情做去了，大概20多分钟就配置好了，同时raspbmc在有路由的情况下DCHP会自动分配地址，raspbmc就会自动联网，至于没有路由的情况我还没试过，估计会需要一些设置，这个找时间求证一下     
![Imgur](http://i.imgur.com/dUwNZs6.jpg)
![Imgur](http://i.imgur.com/vxs3t4J.jpg)
![Imgur](http://i.imgur.com/PkGprSY.jpg)
> 进入系统后会是英文界面，所以第一步设置语言为中文，进入System->Settings->Appearance-
>International->Language，更改English，点击下三角一直到Chinese (Simple)，稍等片刻你看到的估计就会变成方框，这是因为字体的缘故，下一步再设置字体，进入System->Settings->Appearance->Skin->Fonts 这时你看不到中文字体，直接根据位置设置吧，进入Skin后倒数第四项就是Fonts这个选项设为“Arial”，片刻之后就会变成中文的了，这回你就可以随便浏览熟悉了    
设置选项
![Imgur](http://i.imgur.com/PkGprSY.jpg)
> 刚安装好了，里面的推荐扩展都是国外的，天朝下这些貌似没什么用，不过我看到系统里有代理设置，可以搞一个代理帐号试一试翻墙看国外视频，先说说国内可用插件，系统设置->扩展功能->从zip文件安装，浏览文件选择刚刚烧录sd卡后放到/home下的repository.googlecode.xbmc-addons-chinese-eden.zip插件包安装，**安装过程中可能会遇到安装程序格式不完整的错误，这不是什么大错误，可能是下载过程中压缩包内是含入了系统自带文件，我就遇到了这个问题，在windows下载的插件压缩包，无法安装，后来我在linux重新下载了一遍就ok了**   
> 安装好插件以后就能看到优酷、土豆、奇艺这些熟悉的名字了，安装后再扩展视频里就能够通过网络观看了
![Imgur](http://i.imgur.com/Vyjf1MS.jpg)
到此raspbmc在pi上的安装设置就完成了，之后可以尝试通过家庭网络共享访问观看视频，也可以通过本地观看视频（不过sd卡貌似有点小-,-），当然最大的优势还还是这种网络的方式了。看了下优酷下的超清《楚汉传奇》，果然不是盖的啊！唯一不足的是因为512的内存，多多少少在操作上会有些卡和延时，不过一旦进入播放状态就不会有那种问题了，希望pi的下一版硬件继续升级吧！至于发热问题，我看的时间还不够长，所以没有觉察出来
![Imgur](http://i.imgur.com/w8NM4X1.jpg)

----------------------
最后附两个xbmc系统的默认帐号密码  
Raspbmc pi raspberry  
Xbian root raspberry
