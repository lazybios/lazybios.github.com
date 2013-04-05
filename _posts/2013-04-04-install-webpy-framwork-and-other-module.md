---
layout: post
title: webpy、mysql-python、mako安装
categories: 
- webpy
tags:
- webpy
-mysql-python
-mako
---

#####Webpy安装

首先说下setup.py这个文件，该文件一般存在于模块安装中，随模块一起被打包，用来执行安装，webpy模块也不例外，首先下载webpy打包文件，可以到官网下载也可以直接到git上克隆一份源码，安装有俩中方式，一种是直接放到你的开发目录下，另一种用setup.py将万恶不便宜全局安装，具体进入到webpy模块解压后的目录中，执行下面代码
> `python setup.py install`

到这里webpy这个模块就已经安装好了，你可以在你的任意路径下的.py文件中`import web`使用它
#####Mysql-python安装
Webpy装好了，要用mysql，就要用到mysql-python支持，它是个辅助访问mysql数据库的模块，类似于驱动，其windows下安装直接到sourceforge下载驱动，windows下直接就是.exe的，点击执行自动会识别安装到python目录下，并可以全局使用，[下载地址](http://sourceforge.net/projects/mysql-python/files)

#####Mako模板安装
Mako是用python语言开发的开源模板引擎   
1. easy_install脚本下载 [地址](http://pypi.python.org/pypi/setuptools) 下载对应windwos版本的   
2. [Mako官网](http://www.makotemplates.org/)下载mako的tar.gz打包文件   
3. 控制台进入python安装目录下Script中找到1中安装好的easy_install.exe，执行下面代码
> `easy_install.exe x:/../Mako-0.7.3.tar.gz` tar包的存放路径可以随意
