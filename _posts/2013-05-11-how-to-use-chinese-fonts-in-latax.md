---
layout: post
title: Windows下MikTex+TexMaker中文化
categories:
- LaTex
tags:
- MikTex
- TexMaker
- XeLaTex
---

经过这几天的google、baidu发现LaTex的相关资料不是内容杂乱就是内容过时。想解决一个问题，你得在多篇同类文章中找线索，那叫个头大呀-.- 所以我写这篇文章来梳理一下

首先在摆弄LaTex时，要把面这几个概念搞清楚，否则很容易被不同的说辞搞崩溃啊。   
>
+ Tex   TeX 是一种排版语言   
+ LaTex 在Tex指令集合的基础上定义了一整套宏集，方便用户进行排版,可以看成是Tex的一种实现      
+ XeTeX 相较LaTex另一种Tex的实现。支持Unicode 编码和直接访问操作系统字体。   
+ XeLaTex  是XeTeX中的一个命令，用来编译LaTex格式书写的tex源文件，起到兼容LaTex的作用    
+ TextLive  Tex套件，囊括Tex周边的一些程序，类似于编译器 跨平台
+ MikTex windows平台下的Tex套件，与TextLive功能相似，TextLive与MikTex相当于是Tex的两个不同发行版 
+ TexMaker LaTex的书写工具 IDE 用来辅助排版，不是必须的 跨平台可在linux下使用

搞清以上术语仅仅是个开始，下面就要在自己的机器上使用latex了。着重说明下面两个问题 
> 1. MikTex、TexMaker安装与配置   
> 2. XelaTex 中文化使用

####MikTex、TexMaker安装与配置
下载清单    
> [MikTex](http://www.miktex.org/downloado)   
> [TexMaker](http://www.xm1math.net/texmaker/)

按清单顺序安装后，打开TexMaker顶部菜单栏里**选项=>配置Texmaker 里，配置你的pdf查看器**，可以是adobe之类的，其它选项保持默认，你就能书写.tex文件，书写完毕按F1快速构建编译后就可以浏览生成的pdf了

####XelaTex 中文化
这时你的TexMaker可以正确书写和编译.tex文件，但仅限于英文，如果要写中文会输出空白没有显示，到这里我们急切的需要中文的支持，于是就要用到了XelaTex的支持了，前面说过Xelatex是XeTex的一个命令，而XeTex又是Tex一种实现，而这种实现也被包含到了MikTex中了，此时你只需要对TexMaker进行一些相关设置就能使用XelaTex了
> 1. 选项=>配置Texmaker=>编辑器=>编辑字体编码 选择UTF-8   
> 2. 选项=>配置Texmaker=>命令=>LaTex中将`latex -interaction=nonstopmode %.tex`换为`xelatex -interaction=nonstopmode %.tex`  
> 3. 选项=>配置Texmaker=>快速构建=>用户处填写`xelatex %.tex|"C:/Program Files/Adobe/Reader 11.0/Reader/AcroRd32.exe" %.pdf` 这里pdf程序的路径换成你自己的
> 4. 确定

到此配置完毕，通过一个.tex例子来测试下效果。
>
`\documentclass[12pt,a4paper]{article}`   
`\usepackage{fontspec, xunicode, xltxtra}`    
`\XeTeXlinebreaklocale "zh"`   
`\XeTeXlinebreakskip = 0pt plus 1pt minus 0.1pt`    
`\setmainfont{FZShuTi}`    
`\begin{document}`    
`{\noindent Hello,Kitty \\} `  
`世界这么乱，装纯给谁看`   
`\end{document}`    

![Imgur](http://i.imgur.com/1zStrdI.png "output pdf")

解释下上面.tex文件的含义，
> `\usepackage{fontspec, xunicode, xltxtra}`  解决公式显示问题   
> `\setmainfont{FZShuTi}`  设置文本字体    
> `\XeTeXlinebreaklocale "zh"`   下面两行是设置中文自动断行   
> `\XeTeXlinebreakskip = 0pt plus 1pt minus 0.1pt`    

其它项都是LaTex自己的格式，就不做解释了
关于字体，XeLaTex可以使用本系统自带字体，可以通过cmd窗口下运行`fc-list>fonts.log`命令到fonts.log文件中查看看字体对应的英文名称，换到`setmainfont{}`里就可以了。例子中使用的是方正舒体

需要注意的是，这样设置以后，中英文显示都是同一种字体，需要设置某些段落的英文字体格式时，需要单独处理，具体可以查看latex文档

再来说下，网上的一些**靠谱教程**的问题   
[在Windows 7 安裝MiKtex +Cwtex中文環境(一)](http://www.cnblogs.com/dearjustine/archive/2010/04/05/1704495.html)     
这篇文章采用CJK的方法可以实现latex中文化但是设置字体方面很局限只有几种字体，如果需要扩展字体还需要自己动手设置，比较繁琐，没有Xelatex好用。

[Windows 下配置 xeLaTeX + Texmaker 环境](http://intijk.com/?p=295)   
这篇文章采用的是TexLive配置也是可以实现的，但文章中使用的[w32tex](http://w32tex.org/)是2010版的，其官方已经更新到了2013版，不过该文方法是正确的，w32tex软件可以参考[官方](http://w32tex.org/),有中英日三种语言的说明。配置TexMaker可以参考原文也可以参考本文:)    

不过需要提醒的是注意TexLive与MikTex的关系，二者是可以互相取代对方的，所以共存时会有很多问题。装其中一种就好！

以上两篇文章都不错，可以参考，但建议用LaTex完全取代CJK的方式，可以为你省不少事儿。

参考资料：     
[XeTex and Texmaker](http://pomax.nihongoresources.com/index.php?entry=1222289404)   
[Windows 下配置 xeLaTeX + Texmaker 环境](http://intijk.com/?p=295)    
[在Windows 7 安裝MiKtex +Cwtex中文環境(一)](http://www.cnblogs.com/dearjustine/archive/2010/04/05/1704495.html)    
[一个简单的中文模板](http://blog.csdn.net/waksana/article/details/8246116)
