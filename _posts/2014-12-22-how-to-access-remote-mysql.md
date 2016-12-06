---
layout: post
title: 如何设置mysql允许被远程访问
categories:
- mysql
tags:
- vagrant
- mysql
- linux
---

vagran环境配置好了,写代码在host，测试在guest的工作流确实很方便，但是马上就问题来了，每次要查看mysql中内容时候，还需要登陆到guest中通过命令行查看，有的同学举手说，可以安装phpmyadmin通过浏览器查看，确实是个好方法，
但是对我这种非phper，总感觉为了看个mysql，还要装个php环境好累赘。其实优雅的解决方法还是有的，那就是通过mysql GUI client直接对guest中的mysql进行连接查看，GUI客户端，我用的是[Sequl pro](http://www.sequelpro.com/)(Mac环境下最好得mysql可视化客户端)。
guest主机用的是Ubuntu 14.04 具体操作如下:


首先得保证mysql可以被`localhost`之外的主机访问到，这里需要配置`my.conf`文件中的`bind-address = 0.0.0.0`,然后重启mysql服务`sudo service mysql restart`,之后配置当前mysql账号的权限问题了，两种方法：

+ 修改myql的user表,把host一项从`localhost`修改为`%`

```bash

mysql -uroot -p  #进入mysql

mysql> use mysql #进入mysql数据库
mysql> update usre set host = '%' where user = 'root' #修改
mysql> select host user from user; #查看修改结果

```

+ 通过执行授权命令，原理与上面一样，即添加允许访问的host机地址

`GRANT ALL PRIVILEGES ON *.* TO root@my_ip IDENTIFIED BY ‘root_password‘ WITH GRANT OPTION;`

注意上面的`my_ip`处，你可以填写host机的地址，也可以填写一个通配的`'%'`

上面两步操作完之后就可以正常通过host机直接访问guest中的mysql数据了

![graphite-demo]({{site.IMG_PATH}}/sequel-pro.png)

-完-
