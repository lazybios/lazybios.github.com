---
layout: post
title: Centos6.4 Python2.6升级到2.7
categories:
- Linux
- Python
tags:
- linux
- python
---

系统Centos6.4,自带Python版本为2.6.6,安装scrapy和virtualenv遇到版本问题，需要升级到Python2.7以上,查了下资料，还是蛮简单的,下载编译安装，然后改一下

####下载

`wget https://www.python.org/ftp/python/2.7.5/Python-2.7.5.tar.bz2 --no-check-certificate`

具体需要其它版本的，可以到`https://www.python.org/ftp/python/`自行查找，修改一下wget路径即可

####解压编译安装    
`tar -jxvf Python-2.7.5.tar.bz2`   

切换到解压目录   

`./configure  `   
`make all  `   
`make install  `   
`make clean  `   
`make distclean   `   


#####修改python软链接
因为没有卸载2.6，所以目前情况是2.7与2.6共存的状态，默认中终端中执行`python`调用的python2.6的解释器，所以需要修改python软链接：   

`mv /usr/bin/python /usr/bin/python2.6.6`    
`ln -s /usr/local/bin/python2.7 /usr/bin/python`    

这样以后执行`python`就会自动调用`python2.7`了

另外yum脚本依赖的是python2.6版本，修改为2.7会引入新问题，所以需要修改`vim /usr/bin/yum`，将行首的默认执行解释器`#!/usr/bin/python`改为`#!/usr/bin/python2.6.6`

以上步骤做完就算是完成全部升级了~


