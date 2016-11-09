---
layout: post
title: "使用String#[]方法进行正则匹配 | Ruby"
date: "2016-11-09"
---
![ruby_logo]({{site.IMG_PATH}}/ruby_logo.jpg)

Ruby中我们一般用`=~`或`match`完成正则匹配的工作，前者用来验证后者用来捕获匹配，但除了这两个之外，其实用`String#[]`方法也能达到正则匹配的效果，如下:

```ruby
s = "Hello, world!"

# 如果存在返回子串
s['world'] #=> 'world'

# 如果不存在返回nil
s['bye'] #=> nil

# []方法同样支持正则
s[/e..o/] #=> 'ello'

# 捕获字符串中最后一个单词
s[/ ([a-zA-z]*)([^ ]*)$/, 1] #=> 'world'
```

借助`String#[]`方法，我们不仅能做一些比较复杂的子串匹配，也能把它放到条件语句之中，并且从st贴出了得Benchmark结果看，貌似`String#[]`在正则捕获方面要比`match`方法效率更高，所以如果遇到了简单的场景完全可以用这个来代替，但是对于较复杂的正则还是`match`要更灵活。

```ruby
"test123" =~ /1/
=> 4
Benchmark.measure{ 1000000.times { "test123" =~ /1/ } }
=>   0.610000   0.000000   0.610000 (  0.578133)

"test123"[/1/]
=> "1"
Benchmark.measure{ 1000000.times { "test123"[/1/] } }
=>   0.718000   0.000000   0.718000 (  0.750010)

irb(main):019:0> "test123".match(/1/)
=> #<MatchData "1">
Benchmark.measure{ 1000000.times { "test123".match(/1/) } }
=>   1.703000   0.000000   1.703000 (  1.578146)
```

-完-

### 参考引用
+ [http://t.cn/RfA8sjT](http://t.cn/RfA8sjT)
+ [http://t.cn/RfAY3mB](http://t.cn/RfAY3mB)