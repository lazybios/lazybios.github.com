---
layout: post
title: Python中文编码与Unicode
categories:
- python
tags:
- unicode
- 中文编码
---

#####Python编码背景知识
> `x = "hello"`  
> `y = u"hello"`  
> `print type(x)` #<\type 'str'>   
> `print type(y)` #<\type 'unicode'>

从上面的代码可以看出，我们使用的两种字符串赋值方法得到的对象并不相同，前者是String类的实例，后者是Unicode类的实例，前者采用8位大小(即ASCII码)表示一个字符，其最大表示范围0～255，而unicode是一种变长编码其大小根据不同字符会有变化，它的表示范围远远大于ascii码，大约能表示90000个字符，涵盖了世界上大多数语言的文字。在当前版本的python中，官方推荐使用unicode编码处理字符串，之前的String已经停止更新，之所以保留是因为保证python版本间的向后兼容性。

String 与 Unicode的对应关系    
str(args)对应unicode(args) 将传入的args参数变为相应类的实例的字符串    
chr(args)对应unichar(args) 将传入的args参数变为相应的类实例的字符    
其它具体方法可以通过python的自省函数`dir()`去研究

上面说明了python中的两种不同的字符串类型，下面说下unicode编码在python中的使用，python中的编码一般常见utf-8、ascii、uicode的三种，因为unicode是多字节编码，所以其支持多种编码格式，其中就包括了上面3中，正是因为这个优点，unicode编码在python中充当中间‘翻译官‘的角色，即可以将ascii编码先转为unicode再转为utf-8,或者相反，往往出现乱码情况也都是因为在编程过程中使用到了多个介质并且多个介质中的编码格式不统一导致的，例如数据库插入查询时、网页显示、控制台调试这些的情形,因此避免乱码的最好方式就是多平台介质编码格式统一，并且在编程的时候注意以下几点
> + 程序中所有字符串都要采用unicode方式声明即加前缀u"..."   
> + 摒弃string模块改用unicode，str()换为unicode()   
> + 程序内部交流一致使用unicode，只有在需要向外界写出数据的时候，再调用encode()编码函数，相应的，读入数据的时候要用decode()将外界编码转为unicode   

#####常见中文乱码案例
1. 控制调试乱码    
在爬取网页时，得到不同编码的文本，在控制台打印的时候出现乱码，这里涉及4个不同介质，**.py源码文件、爬取网页获取文本、控制台、python解释器**，要想做到不出现乱码就得让这四者做到统一，这里python内部我们用unicode，介质间交流用utf-8.    
相应介质编码修改方式
> + **.py源码文件** 文件的首行或第二行 `# -*- coding: gbk -*-`或者`# coding=utf-8`推荐后者   
> + **爬取网页获取文本** 第三方模块获取,根据不同模块的说明设置python代码内部用设置u为unicode   
> + **控制台** linux下bash是utf-8、windows下cmd是gbk编码，这个一般不好修改，在输出时做相应字符转换即可避免乱码   
> + **python解释器** 解释器修改稍微麻烦一些,将下列代码编辑成一个_env.py文件引入到.py源代码中   
`import sys`    
`if sys.getdefaultencoding != 'utf-8':`    
\>>>>`reload(sys)`    
\>>>>`sys.setdefaultencoding == 'utf-8'`    
上面这种修改方式，自在当时生效解释器内生效，重新解释器执行其它代码，其getdefaultencoding()值又回到了原来的默认ascii

2. 数据库乱码
需保证**数据库服务器Mysql之类、数据库适配器Mysqldb、Web开发框架(python源文件及模板输出html)**三个部分的编码格式相互统一
> + **数据库服务器** 设置正确数据库及所有表的编码校验格式为utf-8    
> + **数据库适配器** 有些适配器支持unicode有些不支持，对于支持的调用其相应方法，对于不支持的就要在二者直接设立中间件来做相应插入和查询返回值编码转换工作,Mysqldb里，需要在connect()传入use_unicode关键字来确认开启unicode编码方式    
> + **web框架与python源码** 这部分要具体参考上面控制台里的一些方法来保证关键部分的编码一致性

#####常见异常：
> + `SyntaxError: Non-ASCII character '\xe4' in file C:\Users\liu\Desktop\test.py on line 1, but no encoding declared; see http://www.python.org/peps/pep-0263.html for details` 

代码中出现非ascii字符以外的符号，需要在源文件中的第一行或第二行做编码声明`# coding = utf-8`

> + `UnicodeEncodeError: 'ascii' codec can\'t encode characters in position 0-1: ordinal not in range(128)`

python解释器默认编码格式为ascii，而代码中出现了非ascii编码，修改办法有俩中，一种是短期的只在一定时间内生效，就是上面修改解释器编码方式那种，另一种是在Lib\site-package文件下新建一个sitecustomize.py文件，内容为:   

>`# coding=utf8`    
`import sys`     
`reload(sys)`      
`sys.setdefaultencoding('utf8')`

这种办法是在系统启动python的时候会自动去修改解释器的默认编码，从而省去手动修改的麻烦
> +`\xe4\xb8\xad\xe6\x96\x87`

控制台如果输出形如上述的字符串，是因为控制台按照ascii码规则打印了utf-8编码的字符串

最后附一个在windows console 总结性的编码实例代码
<script src="https://gist.github.com/lazybios/75d131a842efd7bf258f.js"></script>

参考：  
《Python核心编程》(Wesely J.Chun 著)  
[http://blog.sina.com.cn/s/blog_706ec3f10101bk5e.html]((http://blog.sina.com.cn/s/blog_706ec3f10101bk5e.html)   
[http://www.au92.com/archives/python-requests-chinese-improve-random-code.html](http://www.au92.com/archives/python-requests-chinese-improve-random-code.html)    
[https://github.com/acmerfight/insight_python/blob/master/Unicode_and_Character_Sets.md](https://github.com/acmerfight/insight_python/blob/master/Unicode_and_Character_Sets.md)
