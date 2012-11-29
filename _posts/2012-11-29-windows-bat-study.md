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


####代码运行环境

>+ windows 7
>+ 安装flashpaper，并设置安装目录到path中
>+ 运行时放到任何一个含有待转换文档的目录中，运行即可
>+ 支持pdf\ppt\doc\xls等格式的文档文件

####代码说明




