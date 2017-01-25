---
layout: post
title: "关于===你应该知道的几件事儿 | Ruby"
date: "2017-01-25"
---

与JavaScript里的`===`类似，Ruby里也有一个`===`操作符，不过所不同的是JavaScript里用`===`来验证相等性，而Ruby中的`===`确是用来验证归属性的， `a === b`，的含义可以粗略的描述成表示假设`a`是一个集合，那么`b`属于`a`吗？

一个比较常见的用例是测试一个对象是否是某个类的实例。这也是为什么会把`===`称为`case`比较符的原因。

```ruby
String === "hi"       # True
Object === "hi"       # True
Integer === "hi"      # False
```

这种行为看起来与`is_a?`方法有些类似，不过它还是要比`is_a?`考虑的更多一些。它还可以用来验证某个值是否落在一个给定范围内。

```ruby
(1..100) === 3        # True
(1..100) === 200      # False
```

除了上面这种明确的数值范围比较外，`===`还可以处理正则表达式的验证，因为从某种角度正则表达式所代表的也是一个集合，一个符合表达式条件的字符串集合。这样看可以验证正则就不足为奇了。

```ruby
/abcd/ === "abcdefg"  # True 
```

不过要指出的是，数组类型并没有实现`===`方法，所以在数组对象上调用`===`，实际上调用的是`Object`对象的`===`方法。猜测这样设计应该是因为数组类型是个可变的集合，相比`Set`而言，其集合内容不够明确，没有什么元素可以算的上特定属于某个数组的。

```ruby
[1, 2, 3] === 1      # False
```

### Case语句
了解了`===`的特性，再看`case`语句的好多行为就好理解多了。`case`语句中的比较操作，其实就用的`===`方法。

```ruby
case foo
when "bar"
  do_something()
when "baz"
  do_something_else()
end
```

#### 类型匹配
```ruby
case "hi"
when String
  puts "I'm a string"
end
```

#### 范围匹配
```ruby
case status_code
when (200...300)
  handle_success
when (500...600)
  handle_failure
end
```

#### 正则匹配
```ruby
case "abcd"
when /ab/
  do_something
end
```

### 异常捕获
与`case`语句一样，`rescue`语句在选择要捕获什么异常时候也是用的`===`方法:

```ruby
begin
  do_something
rescue StandardError
end
```

在知晓这一设定的前提下，你完全可以绕过异常的继承体系，自己自定义个实现了`===`方法的异常类，从而让异常捕获的方式更加多变。

```ruby
class FoobarMatcher
  def self.===(exception)
    # rescue all exceptions with messages starting with FOOBAR 
    exception.message =~ /^FOOBAR/
  end
end

begin
  raise EOFError, "FOOBAR: there was an eof!"
rescue FoobarMatcher
  puts "rescued!"
end
```

-完-

### 参考引用
+ [http://t.cn/RxtqI1g]()
