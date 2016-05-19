---
layout: post
title: "Safe Navigation Operator(&.)| Ruby"
date: "2016-05-15"
---

Safe Navigation Operator(&.)是Ruby2.3后引入的新特性，用来更方便的处理异常情况。Matt本人说这是借鉴自Objective-C中nil，它可以接受任何消息，但不做操作任然返回`nil`，这样做的好处是可以忽略掉因为对象为`nil`而导致的异常中断，用起来更像是Rails中的`try`方法。

### Object#try
```ruby
@person.spouse.name if @person && @person.spouse
@person.try(:spouse).try(:name)
```
上面两行代码是等价的，前者通过条件判断来安全的读取出配偶的名字，而后者直接用了try来屏蔽nil的情况。虽然干的一节事儿，但后者的写法显然要简洁不少。

### Safe Navigation Operator(&.)

一样的操作，使用&.操作符后，代码如下:
```ruby
@person&.spouse&.name
```

对比一下，比try又少了一些字符，不过猛的一看，确实有点不适应，在其它语言里这个同样的符号用的是`?.`，但因为Ruby中有带?的断言方法存在，所以只能换一种方式表述，选了&。

二者虽然在功能上相同，但实现上却大不同，前者的try实现上用的是`public_send`方法，这种通过库的方式后天补充的特性，与语言自身内置的特性相比自然要略逊一筹，所以如果你已经升级到Ruby2.3了，那么不妨试一试这个`&.`。

-完-

参考引用

http://t.cn/RqFFdB7
