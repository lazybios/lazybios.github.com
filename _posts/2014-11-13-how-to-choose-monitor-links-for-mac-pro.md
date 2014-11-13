---
layout: post
title: mac pro双显转换器选择
categories:
- max pro
tags:
- osx
- mac
---

就着双11，为自己rmbp加了台23的ips显示器，因为苹果追求轻薄的设计缘故，机身并没有VGA、DVI这样大块头的接口，而是自己设计实现了一个既能传输资料数据，又能传输显示数据的Thunderbolt接口。另外rmbp上HDMI接口也是有的，但是因为我的显示器比较low，只有DVI和VGA接口，所以必须需要一个转换器而不是连接线。大概搜索了下这方面的经验文章，发现有很多不足，大多只有问题，没有答案，于是索性我把自己的选择过程贴出来，希望能帮到有需要的朋友，首先对几个关键名字做下常识普及

#### Mini displayport & Thunderbolt
> 前者只能用来传输视频信号，后者既可以传输数据也可以传输视频信号，二者接口相同，后者兼容前者

#### VGA/DVI/HDMI
> vga 只支持模拟信号        
> dvi 即可以传输数字信号，又可以传输模拟信号       
> hdmi 高清数字信号，同时能传输音频&视频信号       

知道了上面的名词之间的区别之后，基本就能根据特性排个名

+ thunderbolt > mini displayport
+ HDMI > DVI > VGA

但是有上面的基础，貌似还是不行，还需要在各种转换器之间根据自己得实际使用场景做最后选择，根据我的情况：
> 显示器 支持接口 (DVI&VGA)
> rMBP Thunderbolt & HDMI

于是我做了如下尝试

+ mini转VGA接口 + VGA连接线
+ mini转DVI接口 + DVI连接线
+ HDMI转DVI连接线

经过比较以后发现 HDMI转DVI显示效果明显好于其它两个，而且价格上也比上面两个方案便宜不少，一个mini displayport最便宜的也得40rmb，唯一不足的是这样的话更换场地需要携带一根1米长的转换线。且DVI接口也没有VGA那么普及，所以方案1还是必须保留的，最常见的一个场景就是分幻灯片的时候。这样携带就便捷了很多，当然，这回换了vga，显示效果自然会下降一层，不过换来了便携性，还是值得的~

### 最后附张图
![转换器]({{site.IMG_PATH}}/monitor-link.png)
