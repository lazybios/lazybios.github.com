---
layout: post
title: Fedroa下部署Jekyll及Git
categories:
- Blog & Jekyll
tags:
- Git
- Fedroa
- Linux
---

在windows和ubuntu下切换更新blog实在是太麻烦了，因为工作室里有一台实验开发机，所以干脆在windows下通过ssh登录上去操作blog吧，这样一来又要到fedora下重新布置一遍git和jekyll，那就顺道做个笔记，以备回顾...
###Git与Github
#####1. 安装Git 
> `$ yum install git`

#####2. 配置Git 
配置默认用户名： 
> `$ git config --global user.name "Your Name Here"` 

配置默认邮箱地址(跟github用户名一致)：
> `$ git config --global user.name "Your Email Here"`

因为是开发机，公有的，所以最好是不要配置ssh免密码登录了，使用https协议就好，此处设置一个密码缓存就行了具体：
>`$ git config --global credential.helper cache`

设置缓存时间，默认为15min，通过以下方式可以自定义

>`$ git config --global credential.helper 'cache --timeout=3600'`

以上就是github环境的配置，如果你想配置ssh免密码输入，可以github的官方指南[梯子在此](https://help.github.com/articles/ssh-key-setup)

###Jekyll安装配置
我用的是fedora 14，这个版本里有些内置的依赖没有安装，所以网上和[jekyll官方](https://github.com/mojombo/jekyll/wiki/install)给的信息在安装的过程中会有些变化，正确的安装顺序应该是：
#####1. 安装ruby环境能
> `$ sudo yum install ruby-devel`

#####2. 安装RubyGems(需要通过gem 安装jekyll)
> `$ sudo yum install rubygems`

这一步需要提示下，这个过程可能会慢一些，耐心的多等一会儿，不要误以为是死掉了

#####3. 现在可以安装jekyll了
> `$ sudo gem install jekyll`

#####4. 配置语法高亮
> `$ sudo yum install python-pygments`

#####5. 修改markdown语言转换引擎
> `$ gem install rdiscount`

> `$ jekyll --rdiscount`

> 或者也可以在你站点下的 _config.yml 文件中加入以下配置，以便以后每次执行时不必再指定命令行参数：

>`markdown: rdiscount`

至此git及jekyll算是配置完毕，以上所有操作最好统一使用root权限，否则可能会碰到权限问题，之后可以通过测试命令，或者直接克隆些现成的站点来测试一下，具体测试语法：
> `$ jekyll --server`

---------------------------------
**Git 配置补充**

git命令行彩色显示，通过修改～/.gitconfig 文件即可实现此效果:
>`[color]
    diff = auto
    branch = auto
    status = auto
    interactive = auto`

以上修改效果与之前修改默认用户名和邮箱相同，打开该文件你就全明白了，此外可以通过`git config --list查看有哪些配置`


