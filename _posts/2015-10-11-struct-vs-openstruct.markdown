---
layout: post
title: "Ruby: Struct与OpenStruct的区别"
date: "2015-10-11 23:04"
---

Struct

Ruby中的Struct与OpenStruct可以用来构建简单的数据结构，比定义类结构要轻量，类似Hash结构，可以通过方括号的形式来存取，不同的是除了方括号的存取方式，也可以使用点操作符直接存取。

```ruby
Computer = Struct.new :ram, :hard_disk
computer = Computer.new
computer[:ram] = "4 MB"
computer.hard_disk = "500 GB"

computer[:ram]       # => "4 MB"
computer.ram       # => "4 MB"
```

此外Struct还允许定义时带入语法块(Block)

```ruby
Computer = Struct.new(:ram, :hard_disk) do
  def description
    "Computer with #{ram} ram and #{hard_disk} hard disk."
  end
end

computer = Computer.new("4 MB", "500 GB")
computer.description
# => Computer with 4 MB ram and 500 GB hard disk.

computer = Struct.new(:ram, :hard_disk, :processor).new("4 MB")
# => #<struct ram="4 MB", hard_disk=nil, processor=nil>

computer = Struct.new(:ram).new("4 MB", "500 GB", "2.5 GHz", "1024x768")
# => ArgumentError: struct size differs
```

上面的代码，可以看到在定义Struct的同时通过Block语法为其定义了相应的description方法。其行为与对象的方法一样。

通过Struct定义的结构，对传入参数的要求也没有那么严格，当传入参数少于定义时的个数，未传入部分会被置为nil，相反传入超过限定个数时则会引发异常。

OpenStruct

OpenStruct与上面的Struct功能类似，相较Struct只是可以动态向OpenStruct里添加属性，不用预先定义,不过缺点也是有的，不能通过块语法给OpenStruct对象定义方法。

```ruby
require 'ostruct'

computer = OpenStruct.new ram: '4 MB', hard_disk: '500 GB'
computer.ram # => '4 MB'
computer[:hard_disk] # => "500 GB"

# attributes can be created 'on the fly'
computer.screen = "1024x768"
computer.screen   # => "1024x768"
computer[:screen] # => "1024x768"
```


OpenStruct没有包含在Core中，需要使用前require当程序中。同时因为不是Core自带的，所以在性能上也与Struct有所区别，当有性能要求的时候尽可能的去选择Struct。

上面两个类，用在重构和消除依赖上是个不错的选择，当你还没有决定好或者没有足够的信息显示你需要一个复杂的类时候，可以使用上面的两个类去创建与Class行为类似的结构来延续设计，当有足够的信息证明需要一个真正的类时候再将它们替代掉。另外这两个类也可以用在测试当中Mock真正类的行为，以保证测试的隔离性。

-完-

参考引用

http://dwz.cn/1XVc8q
