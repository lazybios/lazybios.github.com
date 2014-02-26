---
layout: post
title: Python的十种惯用法
categories:
- Python
tags:
- python idioms
- python
published: false
---

十种Python惯用法，可以让你的代码更Pythonic

1. *让脚本同时具有可引用性和可独立执行性*

> `if __name__ == '__main__':`  

{% highlight python %}
def main():
	printf('Doing stuff in module',__name__)

if __name == '__main__':
	printf('Executed from the command line')
	main()

{% end highlight %}

可同时在控制台下直接执行，也可以以模块的形式导入后引用   
`import mymodule`    
`mymodule.main()`  

2. *检查逻辑布尔值*

> `if x:`   
`if not x:`

{% highlight python %}
#GOOD
name = 'Safe'
pets = ['Dog','Cat','Hamster']
owners = {'Safe': 'Cat','George': 'Dog'}
if name and pets and owners:
	printf('We have pets')

#NOT SO GOOD
if name != '' and len(pets) > 0 and owners != {}:
	print('We have pets')

{% end highlight %}

3. *尽可能的使用`in`*
> `Contains:`   
`if x in items:`    
`Iteration:`    
`for x in intems:`     

{% highlight python %}
#GOOD
name = 'Safe Hammad'
if 'H' in name:
	print('This name has an H in it!')

#NOT SO GOOD
name ='Safe Hammad'
if name.find('H') != -1:
	print('This name has an H in it!')

{% end highlight %}


{% highlight python%}
#GOOD
pets = ['Dog','Cat','Hanster']
for pet in perts:
	print('A',pet,'can be very cute!')

#NOT SO GOOD
pets = ['Dog','Cat','Hanster']
i = 0 
while i < len(pets):
	print ('A',pets[i],'can be very cute!')
	i += 1


{% end highlight %}

使用`in`可以清楚，精确的检查一个项目是否在一个序列中    
可以在list,dicts(keys)，sets,strings使用，还可在实现的__contains__方法的类中使用


4. *不用临时变量交换两值*
> `a,b = b,a`

{% highlight python %}
#GOOD
a,b = 5,6
print a,b #5,6
a,b = b,a
print a,b #6,5
#NOT SO GOOD
a,b = 5,6
print a,b

temp =a
a = b
b = temp
print a,b # 6,5

避免仅使用一次的temp变量使得命名空间变复杂

{% end highlight%}

5. *使用sequence建立字串*
> `''.join(some_strings)`

{% highlight python%}
#GOOD 
chars = ['s','a','f','e']
name = ''.join(chars)
print name #Safe

#NOT SO GOOD
chars = ['s','a','f','e']
name = ''
for char in chars:
	name += char

print name #Safe
{% end highlight%}
join开销是线性时间，`+`开销是平方使用

6. EAFP 胜于 LBYL
> "it's Easier to Ask for Forgiveness than Permission."    
"Look Before You Leap"    
`try:`   
`except:`   

{% highlight python%}
#GOOD
d = {'X':'5'}
try:
	value = int(d['x'])
except (KeyError,TypeError,ValueError):
	value = None

#NOT SO GOOD
d = {'x':'5'}
if 'x' in d and  is instance(d['x'],str) and d['x'].isdigit():
	value = int(d['x'])
else:
	value = None
{% end highlight %}

在pyhton中抛出异常没有java中那么开销大   
Rely on duck typing rather than checking for a specific type

7. *enumerate方法*
> `for i,item in enumerate(items):`    

{% highlight python %}
#GOOD
names = ['Safe','George','Mildred']
for i,name in enumerate(names):
	print(i,name) #0 Safe,1 George etc.

#NOT SO GOOD
names = ['Safe','George','Mildred']
count = 0
for name in names:
	print(i,name) #0 Safe,1 George etc.
	count += 1
{% end highlight %}

Python2.3+版本都支持enumerate方法    
Python2.6支持使用参数指定从非0位置开始计数

8. *使用列表推导式建立列表*
> `[i * 3 for i in data if i> 10]`


{% highlight python %}
#GOOD
data = [7,20,3,15,11]
result = [i * 3 for i in data if i > 10]
print result #[60,45,33]

#NOT SO GOOD
data = [7,20,3,15,11]
result = []
for i in data:
	if i>10:
		result.append(i * 3)
print result #[60,45,33]
{% end highlight %}

谨慎使用列表推导式，有些情况会使情况变的更复杂，第二种用法反而更明朗清晰

9. *通过键值对建立dict数据结构时使用zip*
> `d = dict(zip(keys,values))`   


{% highlight python %}
#GOOD
keys = ['Safe','Bob','Thomas']
values = ['Hammad','Builder','Engine']
d = dict(zip(keys,values))
print d #{'Bob':'Builder','Safe':'Hammad','Thomas':'Engine'}

#NOT SO GOOD
keys = ['Safe','Bob','Thomas']
values = ['Hammad','Builder','Engine']
d = {}
for i,key in enumerate(keys):
	d[keys] = values[i]
print d #{'Bob':'Builder','Safe':'Hammad','Thomas':'Engine'}

{% end highlight %}

10. 其它

[参看资料](http://safehammad.com/downloads/python-idioms-2014-01-16.pdf)
