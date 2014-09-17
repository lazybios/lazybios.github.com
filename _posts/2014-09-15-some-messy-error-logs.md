---
layout: post
title: 琐碎的错误日志(scrapy&ssh&nginx&uwsgi&yum)
categories:
- Linux
tags:
- linux
- scrapy
- ssh
---

最近一直在和一些 python以及linux相关 配置纠缠不清，顺便记录一下，不过这篇文章有点凌乱...

####Python相关

首先`pip install -v django=1.4.5` 通过`-v`参数可以指定软件的安装版本~，其次pip的安装要通过`wget get-pip.py`在linux的包管理中并没有相应包。

其次安装scrapy这个应用中遇到的问题

`c/_cffi_backend.c:2:20: fatal error: Python.h: No such file or directory`    

{% highlight sh linenos%}
sudo apt-get install python-dev 
{% endhighlight %}


`c/_cffi_backend.c:13:17: fatal error: ffi.h: No such file or directory`

{% highlight sh linenos%}
sudo apt-get install libffi-dev
{% endhighlight %}


`** make sure the development packages of libxml2 and libxslt are installed **`

{% highlight sh linenos%}
sudo apt-get install libxslt1-dev 
{% endhighlight %}

ps：centos debian 区别就是dev 改成devel apt改为yum

####Linux相关
`Agent admitted failure to sign using the key`

删除本地钥匙对，`rm ~/.ssh/id_rsa*`,重新生成后，本地执行一下`ssh-add`，即可~

`Cannot retrieve repository metadata (repomd.xml) for repository updates-released. Please verify its path and try again`

这个错误出现再centos 6.5 yum安装报错，一般这种情况多半是网不通，但最有可能的是源路径有问题，取不到版本号`$releasever`,可以通过取得相应版本号，直接把变量替换为真实值~ 这里即将`$releasever`替换为6.5 yum源修改位置再:`/etc/yum*`,这些都是关于yum软件安装的重要位置~ 

`nginx: [error] open() "/var/run/nginx.pid" failed (2: No such file or directory)`

这个错误是在搭建 nginx+uwsgi+django 环境中执行`nginx -s reload`配置文件的时候遇到的，还是对nginx陌生的缘故，nginx明明还没运行，谈何reload，所以更不会有pid号，自然就会报错，解决 重启`service nginx start`就好了，不用reload

`no python application found, check your startup logs for errors`  

这个错误是再配置uwsgi的过程中遇到的，说明uwsgi与django没有走通，找不到python程序，一般就是路径问题，检查djang_wsgi.py,`chdir`配置是否正确



