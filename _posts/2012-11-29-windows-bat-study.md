---
layout: post
title: Windows Bat 批处理学习（一）
categories:
- BAT脚本
tags:
- Flashpaper文档转换
- 批处理
---

####缘由：

在写一个精品课程的网站，里面有个需求是要展示课程所有的课件ppt，以及课程相关的word文档，这样的需求最好的解决方案就是采用类似百度文库那样以swf格式展示给用户，这样就可以高效的实现在线文档的展示了(主要是flash paper用户广)，大大小小文档一共有将近60个文档，一个一个的打开再用flashprinter转换成swf格式，未免太费时费力了，于是想到个批处理

下面直接贴代码：


{
@echo off
echo initialize... 
echo start convert...
setlocal enabledelayedexpansion
set count=0
for /r %%i in (*.ppt) do set /a count+=1
set uncomplete=%count%
set complete=0
for /r %%i in (*.ppt) do (		 	 
	 echo conveting: %%~ni;
	 flashprinter.exe "%%i" -o "%%~di%%~pi%%~ni.swf"		 	 
	 set /a uncomplete-=1
	 set /a complete+=1
	 echo work state
	 echo ----------	 
	 echo %%~ni%%~xi conveted success
	 echo total:%count%  complete:!complete! uncomplete:!uncomplete!
	 echo.
)
pause
}

####代码运行环境

>+ windows 7
>+ 安装flashpaper，并设置安装目录到path中
>+ 运行时放到任何一个含有待转换文档的目录中，运行即可
>+ 支持pdf\ppt\doc\xls等格式的文档文件

####代码关键点

+ 变量延迟

>bat的机制是命令按整行读取（for中圆括号括住的也当做一行，整体执行），在处理之前要完成必要的处理工作，其中包括对该行命令中变量的赋值。运行“set a=5 & echo %a%之前会提前将a变量的初始值先赋给%a%,这样打印出来的值就肯定不是5，虽然也执行了为a变量赋值为5的命令
>解决方法：
+ setlocal enabledelayedexpansion 开启变量延迟
+ setlocal disabledelayedexpansion 关闭变量延迟
+ 变量延迟对样要用!a!，代替原来的%a%才能生效
这俩句就可以使程序感受到变量的动态变化了，特别是在for循环中经常会运用到

+ FOR循环及其变量

>四种不同迭代变量的for循环，\d目录循环 \r目录文件循环 \l 数字序列变化循环 \f文件解析循环
>迭代变量中的特殊变量，具体可以到百度中查询，其功能是可以把迭代变更分解的显示出来，效果甚妙
>基本语法 for %%variable in (set) do command[command-parameters]，可带以上的4个参数实现不同的循环方式

+ 变量与set命令

>设置自定义变量 set var=0
>简单的计算 a = a + 1 可写为 set \a a+=1

+ 零散知识点

>@echo off @静默echo本身回显 echo off 静默cmd中命令的回显
>echo %a% 打印a变量
>重定向符 >创建覆盖、>>追加、<、>&、<& （不熟悉，用到查文档）
>命令管道符 | 一个命令的输出作为另一个命令的输入
>组合命令 &、&&、|| 多条命令组合起做为一条命令执行 顺序执行，不害怕错误、顺序执行但错误后不知行后面的、第一条失败才执行第二条依次类推
>字符串定界符 双引号"" 路径中或字符串中包含空格或特殊符号可用
>转义字符 ^


