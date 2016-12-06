---
layout: post
title: "《Ruby 元编程》读书笔记 (十六)"
date: "2015-08-28 20:25"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### Self Yield

给一个方法传入代码块时，可以通过yield占位对块进行回调。除了直接调用块外，还可以通过yeild给代码块传递参数，这个self yield的惯用法，其实就是通过yield self，把自身(当前对象)传递给代码块。

#### 使用tap调试方法调用链
```ruby
[‘a’,’b’,’c’].push(‘d’).shift.tap {|x| puts x}.upcase.next
#输出
a
```

这里的tap方法就是利用了self yield技术，将自己传给代码块，从而让我们能打印出当前的状态值，辅助调试方法调用链，避免“火车失事”问题。

Kernel#tap的定义和文档描述如下:
```ruby
tap{|x|...} → obj
```

Yields self to the block, and then returns self.** The primary purpose of this method is to “tap into” a method chain, in order to perform operations on intermediate results within the chain.**

不过自己实现一个tap方法也不难:
```ruby
class Object
    def tap
        yield self
        self
    end
end
```

### Symbol#to_proc方法

Symbol#to_proc方法的目的是位了用更简单的方式来替代一次调用代码块。所谓的一次调用代码块是指那些只有一个参数，且对这个参数只调用一个方法。如下:
```ruby
names = [‘bob’, ’bill’, ‘heather’]
names.map{ | name | name.capitalize } #=> [“Bob”, “Bill”, “Heather”]
```

那么Symbol#to_proc方法又是怎么做到精简的呢？继续往下看
```ruby
names = [‘bob’, ’bill’, ‘heather’]
names.map(&:capitalize) #=> [“Bob”, “Bill”, “Heather”]
```

上面出现了&符号，&符号的含义是： 这是一个Proc对象，我想把它当成代码块来使用。去掉&符号，将能再次得到一个Proc对象。&符号可以用作任何对象，它会调用该对象的to_proc方法来把这个对象转换为一个Proc，之后map方法会将names数组中的每个值作为这个Proc对象的参数进行调用。
```ruby
class Symbol
    def to_proc
        Proc.new { |x| x.send(self) }
    end
end
```

用到了动态派发，to_proc方法会返回一个Proc对象，该对象会在传入的参数上执行:method_name（符号方法，因为动态调用的是self，符号对象本身）。

此外，Ruby中的对象还支持多于一个参数的块，类似inject方法(ps: 这里还不是很熟),见下面代码。
```ruby
[1,2,5].inject(0) { | memo, obj | memo + obj } #=> 8
[1,2,5].inject(0, &:+) #=> 8
```

-完-
