---
layout: post
title: "Flask应用部署小记"
date: "2015-06-27 13:37"
---


### 部署架构
+ Ubuntu 14.04
+ Nginx
+ uWSGI
+ Supervisor
+ Virtualenv


#### Virtualenv

**安装**

`pip install virtualenv`

**使用**

{% highlight sh linenos %}

# 创建虚拟环境
virtualenv env_name
# 使用虚拟环境
cd env_name && source bin/activate
# 关闭虚拟环境
deactivate
{% endhighlight %}


创建源码、日志、配置文件夹，分别用来存放flask app源码、部署日志、nginx和supervisor这些组件的配置文件

`cd env_name && mkdir dev log config`


#### uWSGI
**安装**

在虚拟环境下，执行 `pip install uwsgi`

**使用**
{% highlight sh %}
/path/to/virtual/env/bin/uwsgi -s /tmp/uwsgi.sock -w flask_file_name:app -H /path/to/virtual/env --chmod-socket 666
{% endhighlight %}

uwsgi执行后，生成的sock文件，可能会因为权限问题，无法被nginx正确读取访问，通过`--chmod-socket 666`，修正此问题


#### Nginx
**安装**

`sudo apt-get install nginx`

**配置**

`vim env_name/config/nginx.conf`

粘贴如下内容:
{% highlight sh linenos %}
server {
    listen       80;
    server_name  _;
    client_max_body_size 50M;
    location / { try_files $uri @yourapplication; }
    location @yourapplication {
      include uwsgi_params;
      uwsgi_pass unix:/tmp/uwsgi.sock;
    }
}
{% endhighlight %}

将nginx配置建立软链到sites-enabled文件夹下  

{% highlight sh %}
ln /path/to/virtual/env/config/nginx.conf /etc/nginx/sites-enabled/app_name.conf
{% endhighlight %}

上面几个步骤做好以后，其实就能正常访问了，但是如果需要改动的话，还需要频繁的kill进程，重启进程。通过下面得supervisor来管理uwsgi进程的话，可以很好地解决频繁进程重启和守护的问题。

#### Supervisor
**安装**

`sudo apt-get install supervisor`


**使用**

`vim env_name/config/supervisor.conf`

粘贴如下内容，对应你的项目修改文件

{% highlight sh linenos %}
[program:your app]
command=/path/to/virtual/env/bin/uwsgi -s /tmp/uwsgi.sock -w flask_file_name:app -H /path/to/virtual/env --chmod-socket 666
directory=/path/to/app
autostart=true
autorestart=true
stdout_logfile=/path/to/app/logs/uwsgi.log
redirect_stderr=true
stopsignal=QUIT
{% endhighlight %}

添加完配置以后，通过软链放到supervisor配置文件夹下
{% highlight sh %}
ln /path/to/virtual/env/config/supervisor.conf /etc/supervisor/conf.d/app_name.conf
{% endhighlight %}

启动 `sudo  supervisord`

控制 `sudo supervisorctl`

#### 参考
[http://ewong.me/creating-and-deploying-flask-app-using-uwsgi-nginx-virtualenv-and-supervisor/](http://ewong.me/creating-and-deploying-flask-app-using-uwsgi-nginx-virtualenv-and-supervisor/)

[http://flaviusim.com/blog/Deploying-Flask-with-nginx-uWSGI-and-Supervisor/](http://flaviusim.com/blog/Deploying-Flask-with-nginx-uWSGI-and-Supervisor/)
