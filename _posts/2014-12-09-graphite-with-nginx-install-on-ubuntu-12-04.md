---
layout: post
title: 在Ubuntu14.04上安装Graphite与Nginx
categories:
- graphite
tags:
- linux
- ubuntu
- graphite
---

graphite是一块实时监控软件，其由3部分组成，存储数据的wishper,接受数据的carbon，以及可视化显示的graphite-web,三者合是款性能不错得企业级开源监控软件,目前国内豆瓣，渣浪都在使用~ 下面得安装假设你的系统是刚刚安装的Ubuntu14.04

#### 1. 更新软件包索引与系统

{% highlight sh nos %}

sudo apt-get update
sudo apt-get upgrade

{% endhighlight %}

#### 2. 安装更新graphite依赖
`sudo apt-get install python-django python-cairo python-pip python-django-tagging`

#### 3.安装twisted

因为对于carbon对twisted特定版本有依赖，所以还不能单纯的安装最新版的twisted，通过pip安装

为使twisted顺利编译安装，`sudo apt-get install python-dev`

`sudo pip install "twisted<12.0"`

#### 4.安装nginx与uwsgi以及相应模块(graphite-web是基于django开发的)

`sudo apt-get install -y nginx uwsgi-plugin-python uwsgi`

#### 5.安装graphite套件

`sudo pip install whisper carbon graphite-web`

以上就完成了graphite依赖环境与其自身所有的安装，之后根据实际需求分别进行配置graphite,nginx与uwsgi这三个部分，其实就是基于nginx配置django应用，具体如下:

#### 6.修改配置文件（nginx,uwsgi,graphite,）
##### Graphite
`/opt/graphite/`为graphite的默认安装路径,其中`/opt/graphite/conf/`存放的graphite的配置文件
{% highlight sh nos %}

` cp storage-schemas.conf.example storage-schemas.conf`
`cp storage-aggregation.conf.example storage-aggregation.conf  `
`cp carbon.conf.example carbon.conf`
`cp graphite.wsgi.example wsgi.py`
`cp /opt/graphite/webapp/graphite/{local_settings.py.example,local_settings.py}`
{% endhighlight %}

打开`local_settings.py`修改时区`TIME_ZONE='Asia/Shanghai'`

##### Nginx
{% highlight sh nos %}

#/etc/nginx/sites-available/graphite
server {
   listen 8080;
   charset utf-8;
   access_log /var/log/nginx/graphite.access.log;
   error_log  /var/log/nginx/graphite.error.log;
   location / {
    include uwsgi_params;
    uwsgi_pass 0.0.0.0:8888;
    }
}

{% endhighlight %}

建立软连接到site-enabled启用该VirtualHost

`ln -s /etc/nginx/sites-available/graphite /etc/nginx/sites-enabled/`

##### uWSGI

{% highlight sh nos %}

#/etc/uwsgi/apps-available/graphite.ini

[uwsgi] processes = 2
socket = 0.0.0.0:8888
gid = www-data
uid = www-data
chdir = /opt/graphite/conf
module = wsgi:applicationdpuf

{% endhighlight %}

建立软连接到/etc/uwsgi/apps-enabled使该配置生效
`ln -s /etc/uwsgi/apps-available/graphite.ini /etc/uwsgi/apps-enabled/`


上面即完成了3者的配置，不过最好还是能通过supervisor进程守护去管理uwsgi的启动，这样可以有效的防治uwsgi意外挂掉

##### Graphite(Dajngo)

配置完毕后，就能着手启动graphite工作了,但在这之前还需要同步一下graphite的数据库并修改一些文件的权限，如下
`sudo  python /opt/graphite/webapp/graphite/manage.py syncdb`  与Django中相同，不过要考虑文件路径的权限，否则可能因为权限数据库无法顺利创建出来

修改`/opt/graphite/webapp`与`/opt/graphite/storage`文件权限为`www-data`,因为uwsgi的启动用户是`www-data`

`chown -R www-data:www-data /opt/graphite/webapp/ /opt/graphite/storage/ `

#### 7.启动graphite
至此所有全部搞定，先开carbon，再开uwsgi，最后nginx


{% highlight sh nos %}

/opt/graphite/bin/carbon-cache.py start
/etc/init.d/uwsgi restart
/etc/init.d/nginx restart

{% endhighlight %}

打开浏览器,输入`http://ip:8080`就能看到效果了，如果不能正确出图,就要排查nginx或者uwsgi的问题了，一般这里出错居多，反正我是如此

排错你可能会需要这几个日志
{% highlight sh nos %}

#Nginx日志
/var/log/nginx/graphite.error.log
/var/log/nginx/graphite.access.log

#uwsgi日志
/var/log/uwsgi/app/graphite.log

{% endhighlight %}

#### 注意事项
#####ImportError: No module named defaults
最后补充一个问题，就是django版本问题引起的`ImportError: No module named defaults`错误，因为在django1.6中把这个模块删除了，但是graphite还是1.5的版本，你需要修改的就是所有`django.conf.urls.defaults`改为`from django.conf.urls import patterns, url, include` 大概有十个多这样的文件，另外一个办法是通过源码安装graphite-web,因为该bug已经在graphite的master分支中修复过了，通过二进制安装出现这样的问题，多半是更新没有及时到二进制包种，这样侧面说明了源码安装的好处~，whatever，还是跑起来可以看到画图了，之后就是自己使用statsd，collectd这样的收集数据工具给graphite喂数据了。后续我会写相关案例文章，尽情关注~

![graphite-demo]({{site.IMG_PATH}}/graphite-demo.png)


-完-  
