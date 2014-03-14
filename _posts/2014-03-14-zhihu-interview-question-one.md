---
layout: post
title: 知乎面试题（一）
catgories:
- Python
tags:
- zhihu
- python
---

这是一道知乎的笔试题，来源与[这里](http://liushuaikobe.github.io/blog/2013/07/24/zhi-hu-bi-shi-%28er-%29-ri-zhi-chu-li/)，关于日志处理的，拿来熟练熟练下Python，先贴一下题目：

![题目-日志处理]({{site.IMG_PATH}}/zhihu1.jpeg)

拿到这个题目，首先想到的是正则，也没考虑算法之类的，就是想先把相关信息匹配得到，其实就是主谓宾，待捕获处理的数据是宾语，处理的动作是谓语，计算机是主语，相应的数据到位了，对象找准了，下面就是按题目要求，流程办事，感觉这里最重要的就是正则了，看到引用的博文中强调算法优化，性能问题，感觉有点矫往过正，python本来就已经把一些常用的数据结构做的很到位，只要熟悉特性加以合理运用就好了，每必要在还没写出Version1.0时就开始考虑过多的优化，基本功能实现以后能跑起来，这时才应该把重点放到优化上，当然如果你积累够深厚，当然可以在事前就想到可能会有瓶颈的方方面面，下面是我的思路：

题目要求两个列表，并且这两个列表间还有依赖关系，所以从第一个用户列表下手，根据数据的结构可以写出对应要求的正则表达式，其中使用`()`捕获关键数据，存储当然是用dict了，也就是Hash Table，这样的化在查找的速度上会快一些(这很容易想到，Python就那几种数据结构，放到列表里也不容易掌握数据规律，所以不予考虑！)，同时根据列表不同，分别写了对应两个正则表达式来满足要求

#####正则
{% highlight python %}
#获取用户列表
regex_get_id = r'^\[[A-Z]\s+\d{6}\s+(?:\d{1,2}:){2}\d{1,2}\s*]\s+(\d+)\s+\d{3}\s+GET\s+/topic/(\d+)\s+\((?:[0-9]{1,3}\.){3}[0-9]{1,3}\)\s+\d*\.\d*ms$'
#获取路径列表
regex_get_url = r'^\[[A-Z]\s+\d{6}\s+(?:\d{1,2}:){2}\d{1,2}\s*]\s+(\d+)\s+\d{3}\s+GET\s+(/[a-z]+/\d+)\s+\((?:[0-9]{1,3}\.){3}[0-9]{1,3}\)\s+\d*\.\d*ms$'

{% endhighlight %}

#####程序代码
{% highlight python %}
\#!/usr/bin/python
\# -*- coding: utf-8 -*-

import os
import re
import sys

regex_0 = r'^\[[A-Z]\s+\d{6}\s+(?:\d{1,2}:){2}\d{1,2}\s*]\s+(\d+)\s+\d{3}\s+GET\s+'
regex_1 = r'/topic/(\d+)'
regex_2 = r'\s+\((?:[0-9]{1,3}\.){3}[0-9]{1,3}\)\s+\d*\.\d*ms$'
regex_3 = r'(/[a-z]+/\d+)'

regex_get_id = regex_0 + regex_1 + regex_2
regex_get_url = regex_0 + regex_3 + regex_2

def main(argv):
	if not argv:
		print 'usage: parse_log.py -f <logfile>'
		#sys.exit()


\#generate user id list	
	pattern_1 = re.compile(regex_get_id,re.MULTILINE)
	user_list = {}
	final_user_list = []
	nlist = os.listdir(argv[0])
		
	for name in nlist:
		print '*'*50,name,"*"*50
		name = argv[0] +'/'+ name
		fo = open(name,'r')

		for line in fo:
			m = re.match(pattern_1,line)	
			sub_info_list = []
			if m:
				#print m.group(1),m.group(0)
				if not user_list.has_key(m.group(1)):
					sub_info_list.append(1)
					sub_info_list.append(m.group(2))
					#sub_info_list.append(m.group(3))
					user_list[m.group(1)] = sub_info_list 
				else:
					user_list[m.group(1)][0] += 1 
					user_list[m.group(1)].append(m.group(2))	
		
		#lambda function 				
		for key,value in user_list.items():
			if value[0] < 2:
				del user_list[key]
			elif len(set(value[1:])) < 2:
				del user_list[key]

		final_user_list = list(set(final_user_list).union(set(user_list.keys())))
		fo.close()

	#print final_user_list
	print 'user id list:\t',final_user_list

\#generate access url list

	pattern_2 = re.compile(regex_get_url,re.MULTILINE)
	url_list = {}

	for name in nlist:
		name = argv[0] +'/'+ name
		fo = open(name,'r')
		sub_url_list = []
		for line in fo:
			m = re.match(pattern_2,line)
			if m:
				#print m.group(),m.group(2)
				if m.group(1) in final_user_list:
					if not url_list.has_key(m.group(1)):					
						sub_url_list.append(m.group(2))
						url_list[m.group(1)] = sub_url_list
					else:
					 	url_list[m.group(1)].append(m.group(2))
		fo.close()


	final_url_list = {}
	for user_url in url_list.values():
		user_url = list(set(user_url))
		for url in user_url:
			if not final_url_list.has_key(url):
				final_url_list[url] = 1
			else:
			 	final_url_list[url] += 1

	for key,value in final_url_list.items():
		if value < 2:
			del final_url_list[key]

	print 'access url list:\t',final_url_list
	
if __name__ == "__main__":
	main(sys.argv[1:])

{% endhighlight %}

我按照一个文件夹里放一天的记录，共24个，30个文件夹，我只写了处理一天的部分，30天，可以根据文件夹的数量来for循环处理汇总。

测试用的就是原博主写的测试生成代码，自动生成的模拟数据，不过我发现一个问题，它的那个脚本过于随机，符合结果的数据不多，如果你也打算实现一下，那么建议你在生成文件后自己手动按要求改几个符合条件的数据，看你的程序能不能找出来，否则随机生成你都不知道正确结果有几个，没法判断调试代码

#####参考引用
[http://liushuaikobe.github.io/blog/2013/07/24/zhi-hu-bi-shi-%28er-%29-ri-zhi-chu-li/](http://liushuaikobe.github.io/blog/2013/07/24/zhi-hu-bi-shi-%28er-%29-ri-zhi-chu-li/) 

ps:这文章的url还真心长！:)
