---
layout: post
title: "Ruby: 一次搞懂Reduce，Inject方法"
date: "2015-11-18 21:42"
---

reduce与inject二者是等价的，reduce是inject的一个别名，从可读性上来看reduce更易阅读。 不过这也是相对的，要看你是从什么语言切过来的。先来看看关于reduce\inject的定义:

```
reduce(initial, sym) → obj
reduce(sym) → obj
reduce(initial) { |memo, obj| block } → obj
reduce { |memo, obj| block } → obj
```

再看些示例代码:

```
(1..8).reduce(:+)
=> 36

(1..8).reduce { |sum, num| sum += num }
=> 36

(1..8).reduce(0) { |sum, num| sum += num }
=> 36
```

第一行代码，会对`(1..8)`内的元素执行累加，没有给定`initial`值时，会默认选择调用集合中的第一个元素作为memo值，这里会选择`1`。reduce只返回最后一个memo结果值.后面两行代码通过代码块的形式进行调用，前者没有给初始值，后者给了initial为0的调用。


估计，你还是没有看的怎么明白，不要紧，来看看下面的图示，应该就没啥疑惑了。

#### reduce内部机制演示
下面图示以上面给出的第二行代码为例，通过折叠分别表示`(1..8)`这几个元素，折叠过程表示加和，每次加和的结果写到折叠的背面。以此类推直到不能折叠，也即遍历完集合中的元素。

```
(1..8).reduce { |sum, num| sum += num }
```


![图1]({{site.IMG_PATH}}/fold_1.jpg)

Loop | Num | Sum
------ | ------ | ------
1|1|1


![图1]({{site.IMG_PATH}}/fold_2.jpg)

Loop | Num | Sum
------ | ------ | ------
2|2|3

![图1]({{site.IMG_PATH}}/fold_3.jpg)

Loop | Num | Sum
------ | ------ | ------
3|3|6


![图1]({{site.IMG_PATH}}/fold_4.jpg)

Loop | Num | Sum
------ | ------ | ------
4|4|10

![图1]({{site.IMG_PATH}}/fold_5.jpg)

Loop | Num | Sum
------ | ------ | ------
5|5|15

![图1]({{site.IMG_PATH}}/fold_6.jpg)

Loop | Num | Sum
------ | ------ | ------
6|6|21

![图1]({{site.IMG_PATH}}/fold_7.jpg)

Loop | Num | Sum
------ | ------ | ------
7|7|28


![图1]({{site.IMG_PATH}}/fold_8.jpg)

Loop | Num | Sum
------ | ------ | ------
7|8|36

### 参考引用
+ [http://dwz.cn/2cm8XC]()

