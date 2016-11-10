---
layout: post
title: "强化版的concat与prepend | Ruby"
date: "2016-11-10"
---

![ruby_logo]({{site.IMG_PATH}}/ruby_logo.jpg)

Ruby 2.3之前，我们可以使用`#concat`来向一个字符串或数组中追加元素，并且可以使用`#prepend`向字符串起始位置插入子串。Ruby 2.4中我们仍然可以这样做，不仅如此它们的用法还得到了进一步强化。

### Ruby 2.3
#### String#concat and Array#concat
```ruby
string = "Good"
string.concat(" morning")
#=> "Good morning"

array = ['a', 'b', 'c']
array.concat(['d'])
#=> ["a", "b", "c", "d"]
```

#### String#prepend
```ruby
string = "Morning"
string.prepend("Good ")
#=> "Good morning"
```

Ruby 2.4以前这些方法一次只接受一个参数。

```ruby
string = "Good"
string.concat(" morning", " to", " you")
#=> ArgumentError: wrong number of arguments (given 3, expected 1)
```

### Ruby 2.4
#### String#concat and Array#concat
```ruby
string = "Good"
string.concat(" morning", " to", " you")
#=> "Good morning to you"

array = ['a', 'b']
array.concat(['c'], ['d'])
#=> ["a", "b", "c", "d"]
```

#### String#prepend
```ruby
string = "you"
string.prepend("Good ", "morning ", "to ")
#=> "Good morning to you"
```
可以看到，Ruby 2.4完美的解决了之前只能接受一个参数的问题，并且除了可以接受多个参数外，现在还接受不传入参数的情况。

```ruby
"Good".concat
#=> "Good"
```

### concat与<<的不同
如果只是调用一次，其实两个没啥区别，但如果是连续调用，差异就出来了，如下：

```ruby
str = "Ruby"
str << str
str
#=> "RubyRuby"

str = "Ruby"
str.concat str
str
#=> "RubyRuby"

str = "Ruby"
str << str << str
#=> "RubyRubyRubyRuby"

str = "Ruby"
str.concat str, str
str
#=> "RubyRubyRuby"
```

`<<`连续调用时，表达式从右往左执行，第一个`<<`执行完以后，`str`已经变成了`RubyRuby`，在执行一次就成了`RubyRubyRubyRuby`了。这时其实`concat`更符合我们直观的期待。


-完-

### 参考引用
+ [http://t.cn/RfLqbV3](http://t.cn/RfLqbV3)
