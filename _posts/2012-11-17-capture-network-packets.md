---
layout: post
title: 基于winpcap网络数据包的抓取
categories: 
- 计算机网络
tags:
- C语言
- Winpcap
---

半期考试刚刚结束，后边还有一大摊的事情等待规划和实施，先在此把上次的网络编程实验作业做个小总结吧

####实验内容

>抓取流经网卡的数据包，目标捕获数据数据链路层的MAC地址，网络层的IP地址，以及一些相关数据



####实验规划

>1. 环境 c语言 code::blocks IDE 

>2. winpcap 开发工具包 ([下载地址](http://www.winpcap.org/devel.htm))



####关键点

>+ 开发环境配置

>>1. 首先到官网下载winpcap，一路next安装       

>>2. 配置IDE环境，即添加第三方链接库和设置文件头文件搜索路径，project上右键选择build options设置，linker settings->add,添加开发包下lib目录中的Packet.lib,wpcap.lib 以及Mirosoft SDK下的Ws2_32.lib;Search directories->add 添加头文件搜索路径为winpcap开发包中的incude文件夹



>+ 抓包过程

>>1. 获取网卡适配器

>>2. 打开适配器并设置网卡适配器为混乱模式

>>3. 编译设置过滤器，过滤抓取指定类型数据报

>>4. 分析抓取到的数据报 *分析数据报的过程主要是运用C语言的指针偏移和结构体*



>+ 代码

>>1. 代码有些长就不贴出来了，给出[下载地址]()



>+ 遇到的问题

由于代码高亮插件没整明白，先写在这里，回头补上
