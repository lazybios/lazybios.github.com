---
layout: post
title: "Ruby中的attr_accessor是什么？"
date: "2017-01-03"
---

我们假设你有一个名为`Person`的类。

```ruby
class Person
end

person = Person.new
person.name # => no method error
```

显然我们从未定义过这个`name`方法，所以会报`no method error`异常。让我们现在定义下这个方法。

```ruby
class Person
  def name
    @name # simply returning an instance variable @name
  end
end

person = Person.new
person.name # => nil
person.name = "Dennis" # => no method error
```

现在，可以去读取这个`name`变量，但是这并不意味着我们可以给这个`name`成员变量赋值。这是两类不同的操作(方法)，前者我们称之为**只读**方法，后了我们叫做**写**方法。目前为止我们只有只读的`name`方法，还没有定义过相应的写方法。

```ruby
class Person
  def name
    @name
  end

  def name=(str)
    @name = str
  end
end

person = Person.new
person.name = 'Dennis'
person.name # => "Dennis"
```

好了，通过上面的代码现在我们可以对`@name`实例变量进行读写操作了。但由于这类操作属于高频操作。所以如果照这个写法，我们必然会写出一大堆模板代码。介于此我们需要一个方法能让这类操作变得精简一些。

```ruby
class Person
  attr_reader :name
  attr_writer :name
end
```

但即便如此，我们仍需要分别标识出一个变量的读写方法，这样看起来仍然存在冗余。当我们的使用场景要求我们同时兼具读写方法时，其实我们可以尝试一下`attr_accessor`:方法


```ruby
class Person
  attr_accessor :name
end

person = Person.new
person.name = "Dennis"
person.name # => "Dennis"
```

上面写法除了定义变得精简了许多之外，其它与之前两种写法效果完全相同。并且，与其它方法类似，这两个读写方法可以被用在任意方法中:

```ruby
class Person
  attr_accessor :name

  def greeting
    "Hello #{@name}"
  end
end

person = Person.new
person.name = "Dennis"
person.greeting # => "Hello Dennis"
```

-完-

### 参考引用
+ [http://t.cn/RMZ9qtG](http://t.cn/RMZ9qtG)

