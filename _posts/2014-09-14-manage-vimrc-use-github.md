---
layout: post
title: 用github备份管理vimrc文件
categories:
- Linux
tags:
- linux
- vim
- git
- zsh
---

因为有跨多个操作系统的使用场景，来来回回换vim配置是件很头疼的事情，效率反而会下降，索性用github托管起vimrc文件，用的时候直接clone一份到新系统环境中

vim插件我是用pathogen来做包管理的，因为感觉pathogen这种**需要直接download到`~/.vim/bunder/`下,不需要直接删除**，相比起Vundle这种自动化包管理，更容易被掌控。而且目前来说我使用的插件并不算多，寥寥数个而已...

托管过程即直接把`.vim`文件 与 `.vimrc`文件放到同一目录中，一起push到github repo中。

托管vimrc的思路同样可以放到管理备份gitconfig文件与zshrc这些linux下边的配置文件，当切换一个新环境时，能迅速的切换使用环境~ 

####vimrc地址
[https://github.com/lazybios/vimrc](https://github.com/lazybios/vimrc)
