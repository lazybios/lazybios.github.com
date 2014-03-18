---
layout: post
title: 知乎面试题——去除C语言注释行
categories:
- Python
tags:
- python
- zhihu
---

最近在准备各种面试，继上一道关于python的[知乎面试题](http://freshstu.com/2014/03/zhihu-interview-question-one/),又找到一篇，在[这里](http://ashin.sinaapp.com/article/83/),里面有两道题目，这篇文章主要是第一道，关于C语言注释的问题

#####题目
> C语言中有两种注释风格， / XXX / 和 // XXX , 从一个项目(可以理解为一个目录)中去除所有c文件源代码的注释，并统计每个c文件的注释行数。

与上一道类似，这篇也主要是考察正则和一些Python基本文件操作库的，都是些小功能，所以此外考察的就是python编码习惯，这一点在zhihu里的一个关于知乎面试反馈的问题里，也能看出来.[这里](http://www.zhihu.com/question/20141531)

说说思路吧，首先对于这种字符串操作还是老思路，我称其为“主谓宾”,即找到对象(宾),干什么(谓)，主自然就是代码实现部分了，也就是Python

> 主 python code   
谓 统计，删除   
宾 //单行注释 /**/块注释 .c 源文件其实也能算一个

#####实现代码

{ % highlight python linenos % }
#!/usr/bin/python
# -*- coding:utf-8 -*-

import os
import re
import sys

regex_1 = r'(//[^\n]+)|(/\*.+?\*/)'
def main(argv):
    if not argv:
        print "Usage: stat_c.py folder"
        print "\t python stat_c.py /cproject"
        sys.exit()
    pattern = re.compile(regex_1,re.DOTALL);

    for root,dirs,files in os.walk(argv[0],topdown=False):
        for name in [x for x in files if x.endswith('.c')]:
            print '*'*20,name,"*"*20
            name = root + '/' + name
            fo = open(name,'r')
            complete_file = []
            for line in fo:
                complete_file.append(line)
            content = ''.join(complete_file)
            res = re.findall(pattern,content)
            print 'The total comment lines:\t',len(res)
            print re.sub(pattern,'',content)

if __name__ == '__main__':
    main(sys.argv[1:]) 
{ % endhighlight % }

这里的核心就是写正则规则，开始我的正则用的是`(//\s*.*)|(?s)(/\*(\s*(.*?)\s*)\*/)`,在[regex101](http://regex101.com/r/yJ0oA6)上测试一切正常，但是放到python代码中就有问题了，后来才发现原来python对于模式作用范围的控制有一点bug，到[Stackoverflow](http://stackoverflow.com/questions/22468046/regex-to-capture-the-c-langage-comment-use-python?noredirect=1#comment34176784_22468046)求助，其实可以避开单行模式不要用`.`,将单行赘述的写法改成`//[^n]+`前后就不会有模式冲突的问题了，可以对整个regex单行模式。

剩下的就是读取文件就可以了，用正则就可以了。

这两道题都用了`os`,`sys`,`re`模块，如果要面知乎这些模块不能忽略啊。

#####引用参考
[http://ashin.sinaapp.com/article/83/](http://ashin.sinaapp.com/article/83/)

