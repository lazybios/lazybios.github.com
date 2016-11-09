---
layout: post
title: "Ruby 2.3 中的语法新特性 | Ruby"
date: "2016-11-03"
---

![ruby_logo]({{site.IMG_PATH}}/ruby_logo.jpg)


### Safe navigation operator

`&.`安全操作符(翻译出来好怪)，之前写文章介绍过它([这里](http://lazybios.com/2016/05/safe-navigation-operator/))，用起来类似Rails里的`#try`方法。

```ruby
# Ruby <= 2.2.x
if user && user.admin?
  # do something
end

# Ruby 2.3
if user&.admin?
  # do something
end
```

### Frozen string literals

Ruby 2.2 里的字符串是可变的，如果想要阻止字符串被任意修改需要手动的在字符串结尾调用`#freeze`方法，使用不可变的字符串，可以提高Ruby的执行效率。

在2.3里字符串计划被设计为不可修改的结构，但为了更好的过度暂时用这种快捷方式将代码中所有字符串变为`freeze`状态: 在文件头添加一行注释`frozen_string_literal: true`，像声明文件编码格式那样。

```ruby
# frozen_string_literal: true

str = 'cat'
str[0] = 'b'

# frozen.rb:5:in `[]=': can't modify frozen String (RuntimeError)
#   from frozen.rb:5:in `<main>'
```

### Array#dig and Hash#dig

访问嵌套数据结构的场景大家应该都遇到过，随着层级的加深，写出来的代码也会越来越臃肿，2.3里就特别为解决这个问题给`Array`和`Hash`引入了`#dig`方法。

```ruby
Array
list = [
  [2, 3],
  [5, 7, 9],
  [ [11, 13], [17, 19] ]
]

list.dig(1, 2)    #=> 9
list.dig(2, 1, 0) #=> 17
list.dig(0, 3)    #=> nil
list.dig(4, 0)    #=> nil
Hash
dict = {
  a: { x: 23, y: 29 },
  b: { x: 31, z: 37 }
}

dict.dig(:a, :x) #=> 23
dict.dig(:b, :z) #=> 37
dict.dig(:b, :y) #=> nil
dict.dig(:c, :x) #=> nil
```

### “Did you mean?”

这个特性被很多开发者点了赞，当你因为拼写错误触发`NoMethodError`时，解释器会自动猜测你要敲的目标方法。确实是越来越人性化了，从这一点上看Ruby的确是自带鸡汤，让人快乐。

```ruby
2.3.0-preview1 :001 > "foo bar".uppcase
NoMethodError: undefined method `uppcase' for "foo bar":String
Did you mean?  upcase
               upcase!
```

### Hash “comparison”

Hash结构的比较操作符，当表达式为`a >= b`时，它会检查b中所有的键值对是否存在于a中，根据结果返回布尔值。

```ruby
{ x: 1, y: 2 } >= { x: 1 } #=> true
{ x: 1, y: 2 } >= { x: 2 } #=> false
{ x: 1 } >= { x: 1, y: 2 } #=> false
```

### Hash#to_proc

`Hash#to_proc`会返回一个包含所有键值的lambda闭包，在它上面调用`#call`方法可以返回对应值。

```ruby
h = { foo: 1, bar: 2, baz: 3}
p = h.to_proc

p.call(:foo)  #=> 1
p.call(:bar)  #=> 2
p.call(:quux) #=> nil
```

看起来不如`[]`直接，有点多费手续，但是如果与迭代器结合能一定程度上简化`[]`语法。

```ruby
h = { foo: 1, bar: 2, baz: 3}

# instead of this:
[:foo, :bar].map { |key| h[key] } #=> [1, 2]

# we can use this syntax:
[:foo, :bar].map(&h) #=> [1, 2]
```

### Hash#fetch_values

与`Hash#values_at`类似，可以返回指定键对应的值，唯一与`#values_at`不同，`#values_at`当遇到`key`不存在的时候，会返回`nil`，而`#fetch_values`则是跑出一个`KeyError`的异常。

```ruby
h = { foo: 1, bar: 2, baz: 3}
h.fetch_values(:foo, :bar) #=> [1, 2]

h.values_at(:foo, :quux)    #=> [1, nil]
h.fetch_values(:foo, :quux) #=> raise KeyError
```

### Enumerable#grep_v

类似于命令行中的`grep -v`指令，返回正则反向匹配后的结果。这个方法虽然简单，但却能用到很多场景中。

```ruby
list = %w(foo bar baz)

list.grep_v(/ba/)
#=> ['foo']

list.grep(/ba/)
#=> ['bar', 'baz']
```

### Numeric#positive? and #negative?

判断一个Numberic类型值的正负，这两个方法之前一直属于Rails，现在被纳入到Ruby中了，由此足见Rails的影响力了。

-完-

### 参考引用
+ [http://t.cn/RtH09QS](http://t.cn/RtH09QS)