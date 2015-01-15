---
layout: post
title: 由Haproxy简单配置文件到几个基本概念
categories:
- Linux
tags:
- haproxy
- linux
---

本篇通过一个简单的haproxy配置文件范例进行说明Hapraxy的一些基本概念，下面是完整的配置文件  
{% highlight sh nos %}
# 全局配置
global
log /dev/log    local0
log /dev/log    local1 notice
chroot /var/lib/haproxy
maxconn 20000
user haproxy
group haproxy
daemon
# 默认配置 在后面的设置中对应字段默认配置参数可由“defaults”设定
defaults
log     global
mode    http
option  httplog
option  dontlognull
contimeout 5000
clitimeout 50000
srvtimeout 50000
errorfile 400 /etc/haproxy/errors/400.http
errorfile 403 /etc/haproxy/errors/403.http
errorfile 408 /etc/haproxy/errors/408.http
errorfile 500 /etc/haproxy/errors/500.http
errorfile 502 /etc/haproxy/errors/502.http
errorfile 503 /etc/haproxy/errors/503.http
errorfile 504 /etc/haproxy/errors/504.http
# Haproxy状态信息
listen stats 0.0.0.0:1936
mode http
stats enable
stats hide-version
stats realm Haproxy\ Statistics
stats uri /admin
stats auth admin:admin
# 前端配置
frontend dispatch
bind *:80
default_backend relay_group
# 后端配置
backend relay_group
balance roundrobin
mode http
server relay1 11.11.1.22:80 check
server relay2 11.11.1.23:80 check
{% endhighlight %}

####前端(Frontend)
暴露给用户访问的机器,即haproxy所在的机器，用户的请求通过该前端机根据相应负载均衡算法分发到对应的后端机
{% highlight sh nos %}
# frontend <name> 定义系列监听套接字
frontend dispatch
# 定义前端的监听地址和端口
bind *:80
# 默认后端为 relay_group
default_backend relay_group
{% endhighlight %}
上面没有配置访问控制列表(ACL),一般在这里可以通过ACL访问控制设置符合特定要求的特定路径转发，详细的ACL后面会专门开一篇记一下

####后端(Backend)
处理前端机分发过来得请求的后端server,即真正执行程序的地方，可以是若干个server服务组合
{% highlight sh nos %}
# backend <name> 定义后端服务器设置
backend relay_group
# 负载均衡算法类型设置为 roundrobin
balance roundrobin
# mode 指定协议
mode http
# server <app_name> ip:port check 设置服务地址和端口,并设置健康检查
server relay1 11.11.1.22:80 check
server relay2 11.11.1.23:80 check
{% endhighlight %}
上面设置了一个后端组relay_group，会根据前端机满足的分发要求，根据设置好的负载算法选择合适的server进行分发，其中check字段代表server健康检查，用于自动检测server健康状态，如果发现不可达会自动将其从group重删除出去，以防影响到正常服务

###Haproxy的状态信息
{% highlight sh nos %}
# listen <name>：通过关联“前端”和“后端”定义了一个完整的代理
listen stats 0.0.0.0:1936
# mode 执行协议类型
mode http
# 开启状态统计
stats enable
# 启用统计报告并隐藏HAProxy版本报告，不能用于“frontend”区段
stats hide-version
# 统计页面密码框上提示文本，不能用于“frontend”区段
stats realm Haproxy\ Statistics
# 访问状态信息页面路径 http://localhost:1936/admin
stats uri /admin
# 启用带认证的统计报告功能并授权一个用户帐号，不能用于“frontend”区段
stats auth admin:admin
{% endhighlight %}

####负载均衡算法
Haproxy支持很多负载算法，这里罗列出常用的3种
##### roundrobin
>  基于权重进行轮询，在服务器的处理时间保持均匀分布时，这是最平衡、最公平的算法。此算法是动态的，这表示其权重可以在运行时进行调整生效,，Haproxy默认就是这个

##### leastconn
> 新的连接请求被派发至具有最少连接数目的后端服务器；在有着较长时间会话的场景中推荐使用此算法，如LDAP、SQL等，其并不太适用于较短会话的应用层协议，如HTTP；此算法是动态的，可以在运行时调整其权重。

##### source
> hash算法，并由后端服务器的权重总数相除后派发至某匹配的服务器；这可以使得同一个客户端IP的请求始终被派发至某特定的服务器；不过，当服务器权重总数发生变化时，如某服务器宕机或添加了新的服务器，许多客户端的请求可能会被派发至与此前请求不同的服务器；常用于负载均衡无cookie功能的基于TCP的协议；其默认为静态，不过也可以使用hash-type修改此特性

-完-
