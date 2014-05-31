---
layout: post
title: Git回顾笔记(三)
categories:
- Git
tags:
- git
- notes
---
完整的把[gitimmersion](http://gitimmersion.com/)的课程过了一遍，感觉很有收获，基本的日常是没有问题了，不过课程最后还是抛了几个高级一点的话题，后面会自己search把这些留白部分补上

接着前面的2个笔记

####Merge与Rebase区别
两个命令的最终结果都是对修改的合并，但不同的是merge会使commit tree变的愈加复杂不容易理解，相反rebase则可以用来优化commit tree，将错综交汇的commit变成成线性结构的，更易于理解

不过虽然rebase(变基)可以在外观上使得开发的脉络变得清晰，但是它掩盖了事物的真实发展，所以也会带来一些风险，按照书中的guideline所示，rebase最好不要在公开的repo中使用，否则会给团队开发带来一些麻烦(这个还没有践行0.o,还需要深入理解一下)

####remote分支

服务器上的仓库在本地称之为remote.
`git remote `    
查看所有的remote分支

`git remote show name `    
显示指定remote分支的详细信息 包括fetch,push的url等信息

`git remote add name git@github.com:***.git`   
添加远程仓库路径   

`git remote -v`   
显示所有远程仓库，及其路径

`git remote rm name`    
删除远程仓库


####pull fetch 
git pull <=> fetch + merge     
git fetch 只获取commit，但是不进行合并     


####branch
`git branch -a`    
查看所有的分支

`git branch --track greet origin/greet`     
添加本地分支来追踪远程分支的变化

####bare repo
规定以'.git'结尾repo为bare repository，bare      repository只包含版本库中.git下的文件(用于记录repo历史记录的)，不包括项目源文件，可以用来共享repo，我们从github上clone某个版本到本地其实就是通过bare repo来重新构建本地repository的

`git clone --bare hello hello.git`      
`git clone git://localhost/hello.git network_hello`     
通过bare repo构建hello


####在本地启动git服务
`git daemon --verbose --export-all --base-path=. --enable=receive-pack`     
启动git守护进程，路径为当前目录,以上命名没有权限管理，可以接受任何的push，启动服务后url路径为`git://localhost/hello.git`，把localhost换为局域网中真实的ip地址可以在局域网内访问到repo



