---
layout: post
title: Mac OS上运行Exuberant Ctags
categoires:
- Mac
tags:
- ctags
---

mac下得ctags的版本与Exuberant ctags不同，习惯使用后者，所以要通过编译安装，之后再修改下.bash_profile 

编译前需要确认本机上有gcc那一套东西，下个xcode基本都帮你装好了。



到官网下载ctags源码包 [下载地址](http://sourceforge.net/projects/ctags/)

> tar xzvf ctags-5.8.tar.gz    
> cd ctags-5.8    
> ./configure   
> make    
> sudo make install    

修改.bash_profile，添加一行

`export PATH="/usr/local/bin:$PATH"`
