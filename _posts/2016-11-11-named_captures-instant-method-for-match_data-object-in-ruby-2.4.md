---
layout: post
title: "MatchData#named_captures | Ruby2.4"
date: "2016-11-11"
---

![ruby_logo]({{site.IMG_PATH}}/ruby_logo.jpg)

​Ruby中通过`Regexp#match`和`Regexp.last_match`可以获取到MatchData对象，得到MatchData对象后，用`#names`与`#captures`方法分别获取到正则中分组名以及其对分组所捕获的值。

```ruby
pattern = /(?<number>\d+) (?<word>\w+)/
match_data = pattern.match('100 thousand')
#=> #<MatchData "100 thousand" number:"100" word:"thousand">

>> match_data.names
=> ["number", "word"]
>> match_data.captures
=> ["100", "thousand"]
```

虽然可以分别获取这两个类型的值，但如果想要获取一个捕获分组名称与捕获内容的键值对就比较麻烦了，Ruby 2.4以前的`MatchData`并没有提供相关的方法，只能通过手动构造。

```ruby
match_data.names.zip(match_data.captures).to_h
#=> {"number"=>"100", "word"=>"thousand"}
```

不过好消息是，这不2.4就有了嘛，`MatchData#named_captures`方法会返回一组捕获命名与捕获值得键值对。

```ruby
pattern=/(?<number>\d+) (?<word>\w+)/
match_data = pattern.match('100 thousand')

match_data.named_captures
#=> {"number"=>"100", "word"=>"thousand"}
```

-完-

### 参考引用
+ [http://t.cn/RfUIQ2X](http://t.cn/RfUIQ2X)

