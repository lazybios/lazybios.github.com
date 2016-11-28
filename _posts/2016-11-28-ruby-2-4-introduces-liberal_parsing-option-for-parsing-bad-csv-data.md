---
layout: post
title: "使用liberal_parsing选项对非法CSV格式容错"
date: "2016-11-28"
---

CSV是一种被广泛使用的数据格式，几乎所有的语言都会有一个专门用于解析它的模块。在Ruby中这个模块就是`CSV`类。

根据[RFC 4180](https://tools.ietf.org/html/rfc4180#page-4)中的定义，无法解析含有未转义双引号的CSV数据。解析这类不符合RFC标准的CSV，Ruby的CSV类会报出名为`MalformedCSVError`的异常。


不过Ruby 2.4为CSV类引入了一个名为`liberal_parsing`的容错选项，设置该选项后，即使是面对一些不符合RFC标准的数据，也依然会尝试解析它们而不是直接抛出一个无法解析的异常。

```ruby
# Before Ruby 2.4

> CSV.parse_line('one,two",three,four')

CSV::MalformedCSVError: Illegal quoting in line 1.


# With Ruby 2.4

> CSV.parse_line('one,two",three,four', liberal_parsing: true)

=> ["one", "two\"", "three", "four"]
```

-完-

### 参考引用
+ [http://t.cn/RfY4pH8](http://t.cn/RfY4pH8)
