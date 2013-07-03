---
layout: post
title: shell script变量小记
categories:
- linux
tags:
- linux
- shell script
---

#####shell变量定义
定义变量左右等号不能有空格 `PATH=/bin`   
引用显示需要在前面加$符号，`echo $PATH`   
变量中有空格需要用双引号括住，`Name="Linus Torvalds"`,但双引号中$不该变其含义，仍为变量，单引号内的特殊字符，视为一般字符，双引号中要引用特殊字符需要转义   
`$(command)`与``的作用一样，都是命令面嵌套命令  
追加变量内容，可通过"$var"或${var}累加，`A=123; A="A"456; A=${A}789`  

#####环境变量
查看环境变量 `env` `set`   
`echo $$`   shell的pid
`echo $?`   上次执行完命令的状态码
环境变量 全局变量   
自定义变量 局部变量    

#####变量删除、替换
`# ${path#str}` 从变量内容的最前面向右删除，且仅删除最短那个  
\#  非贪婪模式   \##  贪婪模式   
% 从变量最后面向左删除，且仅删除最短那个   
%% 贪婪模式  
/ 非贪婪模式替换 // 贪婪替换

#####变量检测与替换
`new_var=${old_var-content} \ new_var=${old_var:-content}`   
如果new_var已设置，则保留不变，未设置则，设置为content   
`-content` 默认值   
`:`冒号代表，如果old_var没有设置或设置为空(空格)，或未设置则设置为content   

