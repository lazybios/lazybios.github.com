---
layout: post
title: "Ruby 2.4中的Hash#compact与Hash#compact!"
date: "2016-12-02"
---

处理哈希结构数据时，我们经常会遇到需要过滤掉含有`nil`的键值对的需求，如下:

```ruby
{ "name" => "prathamesh", "email" => nil} => { "name" => "prathamesh" }
```

与数组中的`Array#compact`一样，Rails的Active Support中提供了两个`Hash#compact`与`Hash#compact!`方法专门来处理类似问题。


```ruby
hash = { "name" => "prathamesh", "email" => nil}
hash.compact #=> { "name" => "prathamesh" }
hash #=> { "name" => "prathamesh", "email" => nil}

hash.compact! #=> { "name" => "prathamesh" }
hash #=> { "name" => "prathamesh" }
```

Ruby 2.4中这两个方法已经被纳入到了Ruby语言本身，尽管不算什么大的特性，但这意味着之后无论是在数组还是哈希中，我们不需要任何第三方库就可以轻松实现剔除`nil`值得需求，，也足见Rails与Ruby直接独特的关系。

-完-

### 参考引用
+ [http://t.cn/Rf3vSpe](http://t.cn/Rf3vSpe)