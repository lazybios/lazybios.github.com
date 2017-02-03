---
layout: post
title: "如何修改Nginx日志格式"
date: "2017-02-03"
---

有时我们需要分析一些程序的指标数据，这些指标数据除了可以从运行时获得外，还可以通过解析日志得到。对于Web应用，Nginx提供的访问日志里就蕴藏着大量有用信息。今天这篇要说的就是如果修改Nginx默认日志格式，以便于我们更好的挖掘有效指标。

### 设置方法
编辑`/etc/nginx.conf`配置文件，在日志部分添加下面两段代码，编辑完成后重启Nginx服务即可。

```
log_format main '$host - $remote_addr - [$time_local] "$request" '
                '$status $upstream_response_time $request_time "$http_referer"'
                '"$http_user_agent" "$http_x_forwarded_for" $body_bytes_sent ';
access_log /var/log/nginx/access.log main;
```

代码本身具有自解释性不多说了，简单罗列一下变量的含义:

+ `$host` 访问域名
+ `$remote_addr` 客户端IP地址
+ `$time_local` 访问时间
+ `$status` 访问状态码
+ `$upstream_response_time` 应用返回到Nginx的时间
+ `$request_time` 请求时间
+ `$http_referer` 请求来源
+ `$http_user_agent` 访问客户端
+ `$http_x_forwarded_for` 客户端IP地址
+ `$body_bytes_sent` 返回给客户端大小

-完-

### 参考引用
+ [http://t.cn/RxDpGxb](http://t.cn/RxDpGxb)