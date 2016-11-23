---
layout: post
title: "使用Privoxy桥接Http代理到SOCKS5代理"
date: "2016-11-23"
---

![privoxy]({{site.IMG_PATH}}/privoxy.png)

Shadowsocks算是穿墙届的明星利器，相比各类VPN而言不仅稳定可靠而且安装配置也简单便捷。不过，由于SS使用的是SOCKS5协议，所以在适用的广泛性上不是那么理想，比如各大移动终端、PS4之类的游戏设备都没有内置支持SOCKS5协议，相比而言Http的代理模式却是标配。这篇文章就来介绍一种将Http代理桥接为SOCKS5代理的方案。

### 安装

SS的安装细节就不多说了，不过要知道使用Privoxy的目的是将Http代理转成Sock5协议的代理，所以你的服务端、客户端SS都要预先安装好。

```
brew  install privoxy
```

除了Mac OS X外Privoxy也支持其它平台，不多说了搜一搜就可以找到安装的具体方法。

### 配置
Privoxy的配置文件是`/usr/local/etc/privoxy/config`，任意编辑器打开该文件，修改和添加下面两行。

```
listen-address  0.0.0.0:8118
forward-socks5 / 127.0.0.1:1080 .
```

第一行的意思是监听来自任意地址的8118访问，第二行表示将请求转发给本地1080端口，也就是你本地的SS客户端所开放的端口。

Ok，搞明白了重启就算是生效了，在需要填写Http代理的地方分别填写，Privoxy所在机器**IP地址**和上面的**8118端口**就大功告成了。不过这个方案唯一个缺点就是必须保持运行Privoxy的主机长时间在线，当然你也可以考虑把Privoxy跑到可以执行脚本的路由，或者NAS设备上，不过这些都是后文了。

-完-

### 参考引用
+ [https://www.privoxy.org](https://www.privoxy.org/)