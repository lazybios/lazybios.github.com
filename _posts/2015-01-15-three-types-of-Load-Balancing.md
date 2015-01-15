---
layout: post
title: 三种不同的负载均衡类型
categories:
- Linux
tags:
- Load Balancing
- linux
---

昨天在DigitalOcean上看到三幅介绍负载均衡类型的好图，先把图搬运过来，再做一些小延伸~ 23333

###未使用负载均衡
![未使用负载均衡]({{site.IMG_PATH}}/web_server.png)

这种情况下用户直接与web服务器进行连接，且该服务处于单点得状态，如果we server挂掉了，那么后续的所有连接都不回成功，同样的如果访问用户量比较大，因为服务器繁忙的原因可能有部分用户连接不到服务，会流失大量用户。所以这种简单结构的服务，只适合一些规模比较小的web站点，或者作为原型开发使用。


###四层负载均衡
![四层负载均衡]({{site.IMG_PATH}}/layer_4_load_balancing.png)

由图可以看到提供web server服务的后端机不知一台，并且对外提供的接口也不在直接是服务器本身了，而是又封装了一层前端机做负载均衡，该机器会把用户连接服务器的请求，通过报文中的ip和port两部分，以及服务器本身一些负载均衡算法进行指定分发到后端web server机组中的某一台，因为这里的转发依据是根据网络协议栈中的四层(传输层)进行判断，所以称为四层负载均衡，相应地后面还有基于七层(应用层)的负载均衡。具体的转发过程类似于路由转发请求，负载均衡设备会对用户请求的ip和port地址进行修改，修改为相应后端web server中的某一台地址，相应地也会对web server返回的报文做一定的修改，以掩盖真实地web sever地址

###七层负载均衡
![七层负载均衡]({{site.IMG_PATH}}/layer_7_load_balancing.png)

七层负载均衡的图示很类似四层负载，但不同得地方是其将后端的web server 根据不同内容即`/`和`/blog`分成了两组不同内容的后端服务，也即用户的连接请求会根据用户请求内容的不同分发到不同的后端机中。这里的重点是根据**不同请求内容**,既然要读到具体的内容，那么势必需要先建立起TCP连接，所以这里客户端会分别与前端负载均衡设备以及最终的服务器建立TCP连接，所以原则上这里对负载均衡的设备要求也更高，四层负载均衡中只需要与后端服务器建立连接即可。所以这样说来七层负载中的设备有点类似于代理的性质。


### 参考
[https://www.digitalocean.com/community/tutorials/an-introduction-to-haproxy-and-load-balancing-concepts](https://www.digitalocean.com/community/tutorials/an-introduction-to-haproxy-and-load-balancing-concepts)



-完-
