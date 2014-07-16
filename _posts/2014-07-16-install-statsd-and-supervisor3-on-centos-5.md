---
layout: post
title: CentOS5上运行Statsd与Supervisor3.0
categories:
- DevOps
tags:
- Supervisor
- Statsd
---

最近为若干台Centos5的服务器配置了Statsd，Statsd是一个用NodeJS写得日志收集工具，在数值监控中用作Graphite/Carbon的前端代理。

我配置目标是用Supervisor启动守护Statsd，所以需要配置安装的有node，stats，suervisor，不过中间还有很多坑，慢慢填吧。。。

####系统环境
> 操作系统：CentOS 5.8    
> Python 2.4


#####Git
因为Statsd是通过Github发布的，所以安装也是clone它的版本库，所以第一步就是配置Git


> sudo yum install git   
> git --version 

#####Node
Statsd是用node写得，所以先安装node环境，采用nave.sh安装,先克隆nave.sh到本地

> git clone https://github.com/isaacs/nave.git
> sudo ./nave.sh usemain 0.10.26 #指定安装nodev0.10.26  
> node -v

nave详细用法 参见nave.sh的帮助说明

#####Statsd

> sudo git clone https://github.com/etsy/statsd.git  
> 修改配置文件，参看自带的配置文件范例


到这里statsd就算是配置安装完了，下面部分就是使用Supervisor启动守护statsd

#####Supervisor
Supervisor是一个守护进程，可以帮你启动维护一个后台长时间运行的程序，用Python写得，你可以直接用`yum install supervisor`尝试安装，也可以用包管理工具，但是因为Supervisor 3.0 抛弃了对Python2.5一下的支持，所以Python2.4上安装必须采用一些hack的手法。。。

> yum install python-setuptools    
> sudo easy_install supervisor    

这个时候用supervisord启动多半会报错的，大概意思就是meld3，elementtree出问题之类的,解决方案

>cd /usr/lib/python2.4/site-packages/supervisor-3.0a10-py2.4/egg-info/   
>vim requires.txt   

将下面两行注释掉

>\#meld3 >= 0.6.5    
> \#elementtree    

这样就可以了执行`sudo supervisord`了，但是注意要确保机器是否装了meld3这个html模板包，否则在执行的过程中还是会报`Error: nomodule name meld3`错，因为meld3也是要求Python版本大于2.5所以，所以就不能装最新的版本，两个办法一是尝试yum自动选择安装，运气好得话你会成功！`sudo yum install python-meld3`，如果你像我一样没有好运气，那么没有别的办法自己搜一个早期支持Python2.4版本的源码包或者二进制包，手动安装吧。[这里](https://github.com/lazybios/FileShareRepo)有个0.6.3的meld3版本，要得可以直接download下来,用`rpm -ivh python-meld3-0.6.3-1.el5.x86_64.rpm`安装。

到这里需要的Supervisor就算安装好了，使用`echo_supervisord_conf   `测试下看是否安装成功。 

创建Supervisor配置文件
> echo_supervisord_conf > /etc/supervisord.conf
  
如此一来就可以用Supervisor启动守护Statsd里，如果需要查看守护状态，可以使用`supervisorctl`,交互查询!

完！
