---
layout: post
title: "你的Chrome插件都装到哪了？"
date: "2016-11-15"
---

一个Chrome插件从商店里下载好之后，默认会被安装到用户数据目录，该目录除了存放插件之外，还存有访问历史、书签、以及用户的Cookies数据。不过，默认的存放路径根据系统平台的不同，位置也有差异。

```
Windows XP
Google Chrome: C:\Documents and Settings\%USERNAME%\Local Settings\Application Data\Google\Chrome\User Data\DefaultChromium: C:\Documents and Settings\%USERNAME%\Local Settings\Application Data\Chromium\User Data\Default

Windows 10 / 8 / 7 / Vista
Google Chrome: C:\Users\%USERNAME%\AppData\Local\Google\Chrome\User Data\DefaultChromium: C:\Users\%USERNAME%\AppData\Local\Chromium\User Data\Default

Mac OS X

Google Chrome: ~/Library/Application Support/Google/Chrome/Default
Chromium: ~/Library/Application Support/Chromium/Default

Linux
Google Chrome: ~/.config/google-chrome/Default
Chromium: ~/.config/chromium/Default

Chrome OS
/home/chronos/

```

上面的`%USERNAME%`表示系统的用户名，当然了，你也可以重新修改这个路径，不过需要借助`--user-data-dir`参数，具体怎么修改，自定Google吧，不在本文范畴。

知道了存放路径，还不足以定位到插件的具体位置，因为存放各个插件的文件名是一组32位的字母序列，不过对应关系可以通过Chrome的Extension页面找到，方法如下:

通过地址栏访问`chrome://extensions/`页面，开启`Developer mode`后就可以显示每个插件的ID序列了，这个序列就是存放插件的文件名。

![chrome-plugin]({{site.IMG_PATH}}/chrome-plugin.png)

以Mac为例，这个1Password插件的存放位置在:

```
/Users/%USERNAME%/Library/Application Support/Google/Chrome/Default/Extensions/aomjjhallfgjeglblehebfpbcfeobpgk
```

-完-

### 参考引用
+ [http://t.cn/Rft5Ajh](http://t.cn/Rft5Ajh)
+ [http://t.cn/RftqdQB](http://t.cn/RftqdQB)
