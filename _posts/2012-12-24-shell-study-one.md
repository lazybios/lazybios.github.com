---
layout: post
title: Linux shell 学习记录(一)
categories:
- Linux 
tags:
- linux
- linux shell
---

Linux shell脚本，新近完成一个基本的练习，总结记录一下！


####练习要求：
> 1. 包含一段注释，列出你的姓名、脚本名称和脚本目的；
> 2. 问候用户；
> 3. 显示当前日期和时间；
> 4. 显示这个月的日历；
> 5. 显示当前机器名；
> 6. 显示当前操作系统的名称和版本；
> 7. 显示父目录中的所有文件列表；
> 8. 显示root正在运行的所有进程；
> 9. 显示变量TERM、PATH、HOME的值；
> 10. 显示磁盘当前使用情况；
> 11. 用id命令打印出当前用户组ID；


####实现：
1. 注释部分就是 #
2. 问候用户时通过 变量赋值，即`whoami`赋值给username，**需要注意的是`=`俩边不能有空格**
3. 显示当前日期时间 `date "+%Y-%m-%d %T"`
4. 当前月日历 `cal`
5. `echo $hostname`
6. `uname -svr` 参数 -s 系统名称 -v linux版本 -r 操作系统发行号
7. `ls ../`
8. `ps -u root` 查询root用户当前使用的进程
9. 环境变量的输出直接使用 `echo $PATH`即可，其它类似
10. `df -h -T`  以友好阅读的形式显示磁盘使用情况和类型
11. `id -g`
12. `echo -e "hello world \n"` -e 参数识别转移字符，变量等，变量最好用${varname}形式给出，便于区分

