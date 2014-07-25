---
layout: post
title: SVN错误日志
categoires:
- SVN
tags:
- svn
- sae
---

因为在用渣浪的SAE，所以就用到了SVN，之前一直是用Git，也不知道为什么SAE一直迟迟不支持Git,无力。。。

+ **Error 1**
> `svn: Directory /xxx/xxx\.svn' containing working copy admin area is missing `

`svn st`时看到的文件状态前面显示`~`

####出现原因
这个问题出现的原因是没有通过`svn delete`这样的操作来删除svn中得文件，而是直接通过rm绕过svn删除，这种虽然能起到删除文件的效果，但是会把svn的记录文件搞乱，于是就给你来个错

####解决方案
`rm -rf /xxx/xxx ` 删除掉该文件（如果有用，最好提前备份或移到其它地方先）   
`svn update` 同步到最新状态   
`svn delete /xxx/xxx` 通过svn手段删除待删除文件


+ **Error 2**
> `svn xxxx is already under version controll` 

虽然现实已经在版本控制之下，但是`svn st`时仍然显示为`?`

####出现原因
这个问题的原因是你所提交的文件或目录已经在其他SVN的管理下了，也就是说你提交的文件夹里面已经含有了.svn的目录。需要先把它们删除才能提交。

####解决方案
`find ./ -name .svn`        (显示该目录下所有的.svn)文件      
`find ./ -name .svn | xargs rm -rf`       (删除该目录下所有的.svn)文件     

