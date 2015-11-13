---
layout: post
title: "Rails: alias 与 alias_method 的区别"
date: "2015-11-13 20:06"
---

在之前关于[元编程的笔记](http://lazybios.com/2015/08/note-of-meta-pramgraming-with-ruby-11/)中，有说过alias与alias_method的区别，不过当时只强调了在顶级作用域中(main)不能使用alias关键字来定义别名。这里再将二者之间的差异做个补充。

##### 使用 alias
```ruby
class User

  def full_name
    puts "Johnnie Walker"
  end

  alias name full_name
end

User.new.name #=>Johnnie Walker
```

##### 使用 alias_method
```ruby
class User

  def full_name
    puts "Johnnie Walker"
  end

  alias_method :name, :full_name
end

User.new.name #=>Johnnie Walker
```

#### 差异一：调用时传参形式不同
方法调用时的参数不同，alias_method要求参数是字符串或者符号变量，alias则可以直接使用变量名


#### 差异二:  作用域不同
前面有强调在顶级作用域main中，只能通过alias进行别名定义，而不能使用alias_mehtod是因为在main中调不到`Module#alias_method`方法，而alias是关键字的缘故，所以能正常在main中使用。除了这一点外，还有一点也特别重要，通过alias定义方法别名，其变量`self`的内容是固定不变的，即`self`所代表的时调用处的对象，而`alias_method`方法中的`self`却是在运行时确定的，看下面代码:

##### alias实例代码
```ruby
class User

  def full_name
    puts "Johnnie Walker"
  end

  def self.add_rename
    alias :name :full_name
  end
end

class Developer < User
  def full_name
    puts "Geeky geek"
  end
  add_rename
end

Developer.new.name #=> 'Johnnie Walker'
```

##### alias_method示例代码
```ruby
class User

  def full_name
    puts "Johnnie Walker"
  end

  def self.add_rename
    alias_method :name, :full_name
  end
end

class Developer < User
  def full_name
    puts "Geeky geek"
  end
  add_rename
end

Developer.new.name #=> 'Gekky geek'
```

此外，由于`alias_method`是Module中的一个方法的缘故，可以在程序中对其进行重写，相比关键字`alias`要更加灵活。

-完-

### 参考引用
+ [http://dwz.cn/2aUGAm]()
