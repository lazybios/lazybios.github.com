---
layout: post
title: 如何通过6个简单的步骤使用gdb调试c程序[译文]
categories:
- 译文
- Linux C 编程
tags:
- gdb
- linux c
- 翻译
---

> 这是本人第一篇译作，翻译有点粗糙，见谅，[原文地址](http://www.thegeekstuff.com/2010/03/debug-c-program-using-gdb/)

之前我们讨论过如何编写并编译基本的C语言程序[C Hello World Program](http://www.thegeekstuff.com/2009/09/how-to-write-compile-and-execute-c-program-on-unix-os-with-hello-world-example/)。

在这篇文章里，让我们讨论如何用gdb调试工具通过6个简单的步骤调试C程序。
#####为调试写一个故意含有错误的C程序
为了学习C程序调试，让我们创建一个下面这样一个计算和输出一个数的因子的C程序，并且为了调试这个C程序包含一些错误。
> `$ vim factorial.c`    

<script src="https://gist.github.com/lazybios/577d75d126ef2574fcaa.js"></script>

> `$ cc factorial.c`    
> `$ ./a.out`    
> `Enter the number: 3`   
> `The factorial of 3 is 12548672`   

*译者加`cc`命令调用本机默认编译器进行编译，一般是gcc*

让我们来调试它并且学习最有用的gdb命令
#####步骤1. 用含有调试选项的-g编译程序
用-g选项编译你的C程序。它容许你的编译器收集该程序的调试信息
> `$ cc -g factorial.c`   

#####步骤2. 运行gdb
使用以下命令运行C调试器(gdb)
> `$ gdb a.out`   

#####步骤3. 在C程序内部设置一个断点
> 语法：   
> `break line_number`   

其它格式：
+ break[file_name]:line_number
+ break[file_name]:func_name

设置断点在C程序中你认为有可疑错误的地方。一旦你执行该程序，调试器会停到断点所在处，并且给出你调试提示信息。

所以在开始程序前，让我们先在程序中设置下面断点。
> `break 10`   
> `Breakpoint 1 at 0x804846f: file factorial.c, line 10.`   

#####步骤4. 在gdb调试器中执行C程序
> `run [args]`   

你可以在gdb调试工具中使用run命令开始执行你的程序。你同样可以通过`run args`给出程序命令行参数。我们的例子程序不需要任何的命令行参数，所以让我们使用`run`命令，并且开始执行程序。  
> `run`  
> `Starting program: /home/sathiyamoorthy/Debugging/c/a.out`  

当你执行了该程序，它会执行到底一个断点处，并且给出你调试提示信息。
> `Breakpoint 1, main () at factorial.c:10`   
> `10    		j=j*i;`   

你可以使用下面部分提到其它gdb工具来调试C程序。

#####步骤5. 在gdb调试器中输出变量值
> `语法: print {variable}`    
> `范例:`   
> `print i`   
> `print j`
> `print num`

> `(gdb) p i`     
> `$1 = 1`   
> `(gdb) p j`   
> `$2 = 3042592`    
> `(gdb) p num`   
> `$3 = 3`   
> `(gdb)`   

正如你上面看到的，在factorial.c源程序里，我们没有初始化变量j。所以，它取到了一个垃圾值导致出现一个巨大的因子值。

通过初始化变量j=1修复这个问题，重新编译该程序并执行它

尽管已经修复了factorial.c源程序中那些可能的问题，但是它仍然给出错误的因子值。

所以，先设置断点在第10行，并且执行下节提到的continue命令

#####步骤6. 继续执行，跳过函数执行以及进入函数执行——gdb命令
当gdb在断点处停下来有三种相应的gdb操作可以执行。它们分别是继续执行到下一断点处，进入函数执行，以及跳过函数到下一行。
> + `c`或`continue`:调试器将会继续执行到下一行处   
> + `n`或`next`:调试器执行单行命令不进入函数   
> + `s`或`step`:类似`next`命令，但是并不把函数当作一个当行命令跳过执行，相反会进入到函数定义中一行一行的执行

通过使用`continue`以及`step`命令，我们发现问题是在for循环的循环条件中了没有使用`<=`。所以从`<`改为`<=`这个问题就得到了解决。
######gdb命令快捷键
以下命令是最常用的的gdb操作命令快捷键
> + `l-list`   
> + `p-print`    
> + `c-continue`    
> + `step`     
> + ENTER:直接按下回车将会执行上一次执行过的命令

#####其它gdb命令
> + `l command` gdb命令 l或list在调试模式下打印出源码。用l line-number去查看制定行号代码或用l func_name 查看制定函数    
> + `bt` backtrack——输出所有堆栈帧，或内部COUNT frames   
> + `help`——查看特定gdb主题帮助——`help topticname`    
> + `quit`——从gdb调试器中退出
