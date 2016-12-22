---
layout: post
title: "Mac下反编译APK的方法"
date: "2016-12-22"
---

### 安装
反编译的过程中我们会用到下面三个工具:

+ apktool
+ dex2jar
+ jd-gui

这三个工具都是独立的，所以安装的时候要分别到不同的站点下载。过程比较恼人，但又不得不去，因为它们经常会有更新，如果不能保证是当下最新的工具，很有可能会导致反编译失败。

不过，本文不会对它们的安装方法做过多介绍，只说明一下apktool的安装。安装apktool时，除了要下载到`apktool.jar`包之外，还需要下载一个额外执行jar包的bash脚本，地址在[这里](https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/osx/apktool) (文末也有)，保存后，使用`chmod +x apktool`命令给其赋上可执行权限，然后把它们移动到`/usr/local/bin`处让其可以全局访问到。感觉不够清晰，再罗列一遍。

```sh
wget https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/osx/apktool

mv apktool.jar apktool /usr/local/bin

chmod +x apktool
```

完成后，在终端上能访问到`apktool`即表示成功。因为要有反复下载和解压的操作，所以建议各位给它们创建一个独立的文件夹来管理。其它两个，找到官网下载解压即可，遇到没有执行权限的给加个执行权限。下载地址文末有附。

其中apktool可以帮我们获取到应用的资源文件与配置文件，另外两个dex2jar与jd-gui则是用来生成java代码和浏览java代码的。

### 使用
##### 获取资源文件与AndroidManifest.xml
通过`apktool d apk_debug.apk`指令可以很容易的获取到资源文件以及AndroidManifest.xml，可以用来学习别人的布局思路什么的，不过看了一些以后，发现有好多应用都有自定义控件，所以也不是啥都能看明白。

![decompile-apk-1]({{site.IMG_PATH}}/decompile-apk-1.png)

![decompile-apk-2]({{site.IMG_PATH}}/decompile-apk-2.png)

##### 获取Java代码
资源文件有了，但还想看看人家都用什么类库，代码组织结构啥的，这个时候就需要深入到Java代码里了。反编译Java代码要借助上面提到的后两个工具dex2jar与jd-gui，前者用于获得jar包，后者用户浏览jar吧。其实就两步，如下:

```sh
#解压apk，获取代码中的classs.dex
unzip apk-debug.apk -d apk-debug_code 

#将.dex文件转换成.jar文件
sh dex2jar-2.0/d2j-dex2jar.sh apk-debug_code/classes.dex 
```

上面两步执行完以后，会有一个`classes-dex2jar.jar`文件在当前目录下生成，用jd-gui打开即可。

![decompile-apk-3]({{site.IMG_PATH}}/decompile-apk-3.png)

![decompile-apk-4]({{site.IMG_PATH}}/decompile-apk-4.png)

-完-

### 参考引用
+ [apktool wrappter](http://t.cn/RICFf1H)
+ [apktool.jar](http://t.cn/RZECCHn)
+ [dex2jar](http://t.cn/RICFO9J)
+ [jd-gui](http://jd.benow.ca/)
+ [http://t.cn/R56KIjj](http://t.cn/R56KIjj)
