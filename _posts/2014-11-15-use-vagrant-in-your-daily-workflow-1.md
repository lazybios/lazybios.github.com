---
layout: post
title: Vagrant虚拟化开发环境(一)
categories:
- Linux
tags:
- vagrant
- linux
---

在自己的pc机上做开发测试，随着时间的推移，配置势必会越来越庞杂，总是感觉没有类似vitrualenv这样把开发环境做隔离来得舒服，前段时间看到github上rails核心团队的vagrant
的工作流，使用vagrant管理虚拟机，在自己得实体机上写代码，在虚拟机上跑测试，感觉这样真的很的心应手，虽然自己也是在virtualbox中创建虚拟机，通过ssh登陆做开发，但是创建销毁以及ssh的配置上确实是没有vagrant来得遍历，而且同一个团队共享一个开发环境也没有vagrant来得容易，vagrant直接使用`vagrant package`就可以把一个已经配置好的开发环境打包成`box`进行分发。下面简单介绍下vagrant的安装与配置

### Vagrant安装
+ [下载VirtualBox](https://www.virtualbox.org/wiki/Downloads)
+ [下载Vagrant](https://www.vagrantup.com/downloads)

都是比较简单傻瓜化的安装过程，不做太多说明了，安装好后可以在终端输入`vagrant`执行一下看是否安装成功。之后做些简单设置之后就可以使用了，配置跟在virtualbox中配置虚拟机差不多，只不过
是通过命令行的方式进行操作，涉及的命令行如下:

+ vagrant init   初始化vagrant工作目录
+ vagrant box add [box_name]
> 添加系统镜像到box,box其实就是一个系统环境包，类似与操作系统镜像,存放在vagrant工作目录下的`.vagrant`中，系统镜像可以通过官网提供的[vagrantcloud](https://vagrantcloud.com/discover/featured)下载
>
+ vagrant box list 查看本地已经存在的box

创建配置步骤

```bash

mkdir vagrant
cd vagrant
vagrant init chef/centos-6.5
\#通过[vagrantcloud](https://vagrantcloud.com/chef/boxes/centos-6.5)下载系统镜像，会自动生成Vagrantfile文件,下载镜像时间会比较长~
vagrant up #启动vagrant，如果本地没有box，则会从远程下载

```


### Vagrant 配置

#### 网络配置
网络配置可以根据个人的需求不同配置为private_network(host-only)模式和public_network模式式，前者只能进行局域网操作，后者可以通过桥接网络使得虚拟机也可以联网，前者配置方式，在Vagrantfile中添加如下行

`config.vm.network :private_network, ip: "192.168.0.101"`

这里的ip地址，不要跟你实际局域网里的发生冲突了，当然你也可以使用dhcp方式获取ip，具体自行文档。确定ip后，还可以在host机的`/etc/hosts`中添加一条host记录，使得以后访问guest更加方便,后者直接把:private_newwork 改为:public_network，也可以使用dhcp分配ip，此外还多了一个`:bridge`的语句部分，这个细节可以自己根据需要查阅文档

#### 设置端口转发
`config.vm.network :forwarded_port, guest: 3000, host: 3000`

配置网络转发通过3000端口，这里的guest即你的虚拟机，host为你的实体机,即可以通过实体机的浏览器访问测试虚拟机服务,既可以通过[http://127.0.0.1:3000](http://127.0.0.1:3000)地址，实现上面的工作流

####共享文件夹配置
`config.vm.synced_folder "folder_path_in_host", "folder_path_in_guest"`

默认情况下，vagrant设置的共享文件夹就是Vagrantfile所在的目录，通过上面的设置可以设置额外的共享文件夹


以上是基本配置，设置好以后就可以通过`vagrant ssh`登陆到虚拟机进行日常开发了,至于ssh的细节以及一些其它使用方法，放到下一篇文章中说

### 参考文章
+ [https://github.com/astaxie/Go-in-Action/blob/master/ebook/zh/01.2.md](https://github.com/astaxie/Go-in-Action/blob/master/ebook/zh/01.2.md)
+[http://lovelace.blog.51cto.com/1028430/1423343](http://lovelace.blog.51cto.com/1028430/1423343) 常用操作
+ [http://segmentfault.com/blog/fenbox/1190000000264347](http://segmentfault.com/blog/fenbox/1190000000264347)
+ [http://happycasts.github.io/105-vagrant.html](http://happycasts.github.io/105-vagrant.html)
+ [http://www.williamsang.com/archives/2405.html](http://www.williamsang.com/archives/2405.html)
