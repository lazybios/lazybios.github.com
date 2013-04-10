---
layout: post
title: Python包安装助手easy_install
categories:
- python
tags:
- easy_install
-python
---

easy\_install是由PEAK(Python Enterprise Application Kit)开发的setuptools包里带的一个命令，所以使用easy\_install实际上是在调用setuptools来完成安装模块的工作。它可以很方便的让您自动下载，编译，安装和管理Python包

#####easy_install安装
######Windows
可[官网下载](https://pypi.python.org/pypi/setuptools)对应的exe，执行安装，会自动安装到python安装目录下的Scripts路径下，之后在系统环境能变量Path中添加该路径就可以全局使用easy_install命令了

######Linux
+ 通用
> 1. `wget http://peak.telecommunity.com/dist/ez_setup.py`
> 2. `python ez_setup.py`

+ Ubuntu
> `sudo apt-get install python-setuptools`

######使用  
之前安装Mako模板的时候也有用过该命令，即`easy_install 模块包路径`，其也可自动搜索PyPI(Python Package Index),以查找最新版本模块并安装，如果要卸载某模块，可通过`easy_install -m package-name`,此操作会从easy-install.pth文件里把package-name的相关信息抹去，剩下的egg文件，可以手动删除

通过easy_install安装软件，相关安装信息会保存到easy-install.pth文件里
> `Windows：C:/Python25/Lib/site-packages/easy-install.pth`    
> `Linux：/usr/local/lib/python25/site-packages/easy-install.pth` 

参考：
[http://www.juziblog.com/?p=365001](http://www.juziblog.com/?p=365001)






