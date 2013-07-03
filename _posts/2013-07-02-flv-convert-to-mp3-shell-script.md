---
layout: post
title: ffmpeg批量flv转mp3 shell脚本
categories:
- linux
tags:
- ffmpeg
- shell script
- flv2mp3
---

经常看类似《罗辑思维》、《晓说》之类的节目，讲的内容还不错，但是观感上有些欠缺，如果是视频，放到手机上又没法后台，费电不说还得拿出整块时间盯着屏幕，后来想想干脆转成mp3格式，当音乐听了，时间和空间上也更灵活了。最近接触了下ffmpeg，所以决定写个shell脚本批量转了。

思路就是：
> + 用flvcd硕鼠，批量下载了，获取flv格式文件
> + ffmpeg 批量脚本转mp3

这篇文章针对写批量flv2mp3 shell脚本过程遇到的一些问题，做个详尽的记录，以备后查

######\#!/bin/bash
声明script使用的解释器类型，通过这个声明可以使脚本执行时自动加载相应解释器的环境变量，bash对应的环境变量位于`～/.bashrc`中

######export
自定义变量转环境变量，因为bash中执行的命令是以bash衍生子进程的方式进行，子进程会继承父进程的环境变量，不会继承父进程的自定义变量，如果想要传递自定义变量到子进程中，可以用`export var`的方式将自定义变量加入到环境变量中,用下列方式可测试该过程
> `temp=1234`   
> `env`   
> `export temp`   
> `env | grep 1234`

######$?、&&、||、$0\$1\$2...
`$?` 保存上次执行命令结果   
`&&` 与  `||` 或     
`&&`与`||`可组合为条件判断句式：`comd1 && comd2 || comd3` 与 c语言的 `comd1 ? comd2 : comd3`等价   
`$0\$1\$2` 与c语言相同，0表示程序本身，1、2... 标识参数1，参数2... `shfit n` 可以略过指定个数的参数

######shell脚本执行方式
修改script脚本权限为rw，即可通过`./script.sh`执行，修改命令`chmod a+x script.sh` ,该方式脚本的执行是在bash的子进程中执行，子进程内的修改不会作用到父进程中  
`source script.sh` 在仍在父进程中执行，脚本对于bash来说相当与上下文函数，即脚本对某一变量做修改退出后，在bash中访问该值是可以访问到的，这也是为什么某些rc配置文件修改后要用source执行一下生效的缘故，可以免去注销系统。

######shell中的$IFS
IFS,为Internal Field Separator (内部字段分隔符) 缩写，该值用于设定间隔字符，默认为空格，但当你要遍历一个目录下的文件时，碰到了文件名中有空格是一件相当头疼的事情，如果这时临时修改$IFS值，对于解决这个棘手的问题会有很大的帮助，处理完毕在恢复元值即可。

就是这些了，至于语法细节，找本书参考一下就可以了，最后附上小脚本的代码:
{% highlight bash %}
#!/bin/bash
#Program:
#    convert flv to mp3
#History:
#2013/07/02 lazybios@gmail.com First release
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin:/usr/local/ffmpeg/bin
export PATH

filelist=$(find *.flv)
OLDIFS="$IFS"
IFS=$"\n"
for filename in *.flv
#$(find -iname *.flv)
do
	ffmpeg -i $filename -acodec libmp3lame -ab 128k mp3/"${filename%.*}".mp3
done
IFS=$OLDIFS
{% endhighlight %}

使用方法：直接放到，存放flv目录下，执行即可







