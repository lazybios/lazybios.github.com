---
layout: post
title: "Set#compare_by_identity | Ruby 2.4"
date: "2017-01-12"
---

在Ruby中，我们可以使用`Object#equal?`方法去判断两个对象是否完全相同，同时Ruby还有一个叫`Object#eql?`的方法，它用来判断两个对象是否含有相同的值。

```ruby
str1 = "Sample string"
str2 = str1.dup

str1.eql?(str2)     #=> true

str1.equal?(str2)   #=> false
```

通过`object_id`可以清楚的看出`str1`与`str2`是两个不同的对象。

```ruby
str1.object_id      #=> 70334175057920

str2.object_id      #=> 70334195702480
```

`Set`对象中的数据其中一个特性就是**互异性**，在判断两个对象是否相同时，Ruby默认采用的就是`Object#eql?`方法，而不是`Object#equal?`。

但有时你可能恰好想在一个集合里存放两个**字面值相同但对象ID不同**的变量时，这在Ruby 2.4之前是没法通过内置`Set`实现的。

#### Ruyb 2.3

```ruby
require 'set'

set = Set.new           #=> #<Set: {}>

str1 = "Sample string"  #=> "Sample string"
str2 = str1.dup         #=> "Sample string"

set.add(str1)           #=> #<Set: {"Sample string"}>
set.add(str2)           #=> #<Set: {"Sample string"}>
```

Ruby 2.4中引入了一个叫`Set#compare_by_identity`的方法，可以修改Set判断两个对象是否相同的标准，也就是用`object_id_id`作为判断依据，即`Object#equal?`。

#### Ruby 2.4
```ruby
require 'set'

set = Set.new.compare_by_identity           #=> #<Set: {}>

str1 = "Sample string"                      #=> "Sample string"
str2 = str1.dup                             #=> "Sample string"

set.add(str1)                               #=> #<Set: {"Sample string"}>
set.add(str2)                               #=> #<Set: {"Sample string", "Sample string"}>
```

#### Set#compare_by_identity?
除了修改Set的判断标准外，还可以用`Set#compare_by_identity?`来查看一个Set对象采用的是那种判断标准。

```ruby
require 'set'

set1= Set.new                          #=> #<Set: {}>
set2= Set.new.compare_by_identity      #=> #<Set: {}>

set1.compare_by_identity?              #=> false

set2.compare_by_identity?              #=> true
```

-完-

### 参考引用
+ [http://t.cn/RMKChpG]()