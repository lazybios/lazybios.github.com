---
layout: post
title: "《Ruby 元编程》读书笔记(十五)"
date: "2015-08-28 16:33"
---

前面介绍了Ruby元编程中的各种黑魔法，今天这个Topic被作者归纳为**惯用法**，看了以后帮助巨大！这些用法还真是经常出现在各种Ruby大神的代码里，Ruby-China的源码中就不少,不啰嗦进入正题！

### 拟态方法

拟态方法其实就是我们常见类似puts、private以及public这些方法，它们将自己伪装成类似关键字的样子，但其实它们都是Ruby中的方法。在Rails中也有大量的这样用法，比如link_to这样的方法，常常让人搞不清楚这个方法到底可以接收多少个参数？其实它的定义是这样的:

```ruby
link_to(name = nil, options = nil, html_options = nil, &block)
```

之所以感觉它的参数多的原因是，其省略了hash参数的花括号，**Ruby 中如果最後一個參數是 Hash 的話，它的大括號是可以省略的**，所以你才见到link_to参数总是数不完。

### 空指针保护

相信有读过大神代码的同学，一定遇到过这样的赋值语句吧a ||= [], 这里的重点可以放到||=上，特别是在Rails中通过参数对变量赋值什么的。其实它的含义可以用这个表达式表示a || (a = []) ,这里利用了布尔运算的短路求值。等价于下面这段代码：

```ruby
if defined?(a) && a
    # 如果a已经被定义了，且既不为nil也非false，返回a
    a
else
    # 如果a未定义，或未nil，或为false，返回空数组
    a = []
end
```

上面代码，明显表示出，a ||= []这样的表达式，除了能确保变量初始化以外，还能确保其值不为nil和false。所以，不应该在变量值可能是nil或false的情况下使用**空指针保护**

**空指针保护**常用于初始化变量,可以取代initialize方法。如下面代码:

```ruby
class C
    def initialize
        @a = []
    end
    def elements
        @a
    end
end
```

上面代码通过，initialize方法对实例变量@a进行了初始化，确保在调用C#elements方法时，@a是已经初始化的。同样的结果，使用**惰性实例变量**技巧，可以让代码更精简

```ruby
class C
    def elements
        @a || = []
    end
end
```

只有在C#elements被调用的时候，才初始化实例变量@a。

-待续-
