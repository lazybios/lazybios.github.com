---
layout: post
title: Django的部署(Supervisor+Gunicorn+Virtualenv)
categories:
- Django
tags:
- python
- nginx
- gunicron
- django
---

首先来说说这个架构的情况，分别用到了Supervisor，Gunicorn以及Virtualenv，当然还有django自己，数据库因为我的应用比较轻量级只是用来存放几个配置文件而已，所以我用的同样比较轻量的sqlite，分别说下这几个组件的功能吧

+ Supervisor 是一个python实现的守护进程的应用，可以帮你管理你的进程，当进程莫名挂掉可以帮你restart，保证服务持久在线
+ Gunicorn 全名叫(Green unicorn)，来源于Ruby的Unicorn项目，一款Pyhton的WSGI(web server gateway interface) HTTP服务器，采用pre-fork工作模式
+ Virtualenv 允许你可以在同一台机器上安装同一款软件的不同版本，即一套python虚拟环境系统，可以将若干不同python 包隔离安装

上面几个部分的关系:

```
the web client <-> the web server <-> the socket <-> gunicorn <-> Django
```

Supervisor与Virtualenv 都是管理工具，不是应用必须得


###安装
Supervisor的安装方法以前，写过一篇类似的文章，[在这里](),这里就不多说了，一会儿会在配置部分，贴一下具体配置，需要说明的一点，修改了配置文件后，可以通过`supervisorctl reload `来载入新配置并重启所有子进程

#####Gunicorn
Gunicorn通过`pip install gunicorn`可以直接安装，安装好后可以用下面代码测试一下:

```sh

def app(environ,start_reponse):    
    data = "Hello,World!\n"    
    start_response("200 OK",[("Content-Type","text/plain"),    
                    ("Content-Length",str(len(data)))     
                  ])    
    return iter([data])    

```

在相应文件目录下执行 `gunicorn -w 4 test:app --bind 127.0.0.1:8000` 然后到浏览器中访问`127.0.0.1:8000`看到hello，world即为成功~


#####Virtualenv
同样可以使用`pip install virtualenv`进行安装，之后的管理通过`virtualenv env`来创建新的虚拟环境，创建后需要到创建的环境中`bin`目录下，使用`source activate`激活创建的环境，之后系统就会自动进入到新的虚拟环境中，再虚拟环境中可以使用virtualenv自带的pip或者easy_install来安装新的包，这时所有安装的包都会装到新环境的`lib\site-package`中，而不是原系统中，所以当你要使用这些虚拟环境的包时，必须确保你处于激活该环境的状态下，要退出虚拟环境只需要再任何路径下执行`deactivate`，就可以回到系统环境中，这时使用pip之类的包管理软件安装则会装回系统环境中

创建一个django的虚拟环境

```sh

virtualenv dev    
cd dev   
source bin/activate   
pip install -v django=1.4.5  
django-admin.py startproject myapp   
cd myapp    

```


###配置
进入到django应用主目录中，执行

`gunicorn myapp.wsgi:application --bind 127.0.0.1:8000 --pid /tmp/gunicorn.pid --daemon` 

上面bind 表示将ip：port绑定到服务器上，通过这里的ip和端口号就能正确的访问到应用，pid 参数指出的是记录gunicorn进程号文件存放位置，这样我们可以手动或者使用脚本在需要停掉gunicorn时，根据pid进程号杀掉该进程，daemon 表示的改进程做为daemon运行再后台

通过上面的命令你就可以顺利的启动django服务了~ 但是这还不够，应为这个进程很可能因为一些意外原因挂掉，不够保险，这时我们使用上面介绍的Supervisor的方式启动gunicorn就稳妥多了，具体方法是,使用`echo_supervisord_conf > /etc/supervisord.conf`生成配置文件样本，然后根据做具体修改，最重要的是要配置`program`部分，代码如下

```sh

[program:gunicorn]     
command=/opt/dev/bin/gunicorn myapp.wsgi:application --bind 127.0.0.1:8000 --pid /tmp/gunicorn.pid      
directory=/opt/dev/code/myapp
autostart=true

```


做完以上配置后，我们需要启动supervisord程序，使用如下命令`supervisord -c /etc/supervisord.conf`，根据读入的配置文件，会自动启动gunicorn，通过查看进程`ps -aux | grep gunicorn`看我们的服务器是否已经启动，如果已经启动了，我们就可以通过ip在浏览器中看到，正常运行的页面，不过这里要注意静态文件的设置，如果你启用了admin管理后台，那么你需要把admin的css，js等静态文件移到你所设置的静态文件目录中去


其实上面gunicorn里还有好多的配置，比如work数，日志文件存放路径等可以配置，这些内容就需要你根据自己的实际情况到官方文档里慢慢的挖了，supervisor也有一些其它的设置，比如说supervisorctl配置，还有stop进程的模式等，另外你也可以试一试把应用通过`kill`命令杀掉，看supervisor能不能帮你重启，判断一下你的配置是否正确

wsgi: WSGI是一种Web服务器网关接口。它是一个Web服务器（如nginx）与应用服务器（如uWSGI服务器）通信的一种规范。

###参考引用   
[Setting up Django and your web server with uWSGI and nginx](http://uwsgi-docs.readthedocs.org/en/latest/tutorials/Django_and_nginx.html)    
虽然是介绍uWSGI的，但很多地方可以借鉴    
[五步教你实现使用Nginx+uWSGI+Django方法部署Django程序](http://django-china.cn/topic/101/)     
解释了部署过程中的很多概念，行文也比较清晰