---
layout: post
title: Windows文件名在Linux下乱码问题
-categories:
- Linux日常
-tags:
- linux
- encoding
---

Windows下创建的文件在Linux打开会有编码问题（包括内容与文件名），因为二者编码格式不同。解决方法是利用一款名为`convmv`的命令行格式转化工具

######安装
> `sudo apt-get install convmv`

######使用
> `convmv -f cp936 -t utf8 -r -notest *`

-f 源文件编码格式   
-t 目标编码格式  
-r 递归执行   
-notest 直接进行转换，而非仅仅打印出结果    
* 带转换文件，可以是单个，也可以对一个文件夹里的内容直接用通配符    

看到一些网上的文章发现有的说还得下个7zip，这个与解码无关仅仅是个解压工具，完全可以用unzip之类代替，解压后转换所得文件即可，文件少了用文件名，多了为方便直接用通配符！更多用法`man convmv` :)


