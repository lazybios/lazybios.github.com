---
layout: post
title: Git回顾笔记(一)
categories:
- Git
tags:
- git
- notes
---


一直用git来维护自己blog，但突然发现自己一直只在一个分支下工作，完全没有利用到git的强大之处。所以有了重新系统学习git的念头。

我的学习路线如下：
> 参考书： [Pro Git](http://git-scm.com/book)  [中文](http://git-scm.com/book/zh/)
实战指南： [GitImmersion](http://gitimmersion.com)

这里是学习过程中的一些笔记碎片

####配置gitconfig
姓名&邮件
`$ git config --global user.name "Eric"`
`$ git config --global user.email someone@example.com`
跨平台的换行符
`git config --global core.autocrlf input`
`git config --global core.safecrlf true`
为常用操作设置别名
在$HOME/.gitconfig中追加如下代码
>[alias]
    co = checkout
    ci = commit
    st = status
    br = branch
    hist = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
    type = cat-file -t
    dump = cat-file -p

####查看log
`git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short`
>   --pretty="..." 定义log输出格式
    %h hash值的缩写
    %d commit上的一些装饰 (e.g. branch heads or tags)
    %ad 提交日期
    %s 提交注释
    %an 提交者用户名
    --graph 显示commit的继承关系(树形关系)
    --date=short 设置精简的日期格式
    
`git log --pretty=oneline --max-count=2`
单行显示log，并设置最大显示数量


####特殊符号.&^&~
`.`
`git add .` 中的`.`代表当前目录，在子目录中执行该语句不会把上层目录中其它目录中的修改提交到stage中
`^`
`git tag someversion^` 表示someversion的父版本
`~`
`git tag v1~n` 表示v1的第n个祖先


####Git中的一些关键字
HEAD 其表示的是当前分支，可以用来相对操作当前分支的前后分支
master 默认主分支

####git tag
使用commit后生成的哈希值可以用来检出对应的版本，仅使用hash值中的前七位就可以唯一的定位一个commit或tag

查看当前版本库中的tag,tag可以看作是某个发行版的代称
`git tag`
`git tag -d tagname`  删除指定分支
rest到某个分支，只要之前的tag不删，那么其commit也会存在，使用`git hist --all`还是可以看到的

####事件回滚
撤销未stagging的改变
`git checkout filename`
撤销commit前的改变(已进入stage)
`git reset HEAD filename`
撤销commited的修改
`git revert HEAD --no-edit`

####Other Tips
严格的区分每次提交的内容，不要把几个逻辑上不相关的部分作为一次提交，确保每次commit都只是一个功能，对于不是同一个逻辑部分的内容要有选择的add和commit，较大的功能要分为若干功能进行开发提交

