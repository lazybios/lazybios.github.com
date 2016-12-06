---
layout: post
title: "《Ruby 元编程》读书笔记(六)"
date: "2015-08-21 19:45"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 类的返回值

像方法一样，类定义也会返回最后一条语句的值:
```ruby
result = class MyClass
    self
end
result #=> MyClass
```

### 当前类

与当前对象self一样，同时还存在**当前类（或模块）**

Ruby中并没有类似当前对象self一样的明确引用，不过在追踪当前类的时候，可以遵循下面几条:

+ 在程序顶层，当前类是Object，这是main对象所属的类。
+ 在一个方法中，当前类就是当前对象的类。(在一个方法中用def关键字定义另一个方法，新定义的方法会定义在self所属的类中。因为Ruby解释器总是追踪当前类(或模块)的引用，所以使用def定义的方法都成为当前类的实例方法了)

#### 示例代码
```ruby
class C
    def m1
        def m2; end
    end
end

class D < C; end

obj = D.new
obj.m1

C.instance_methods(false)   #=> [:m1, :m2]
```

+ 当用class关键字打开一个类时(或module关键字打开模块)，那个类称为当前类。

### class_eval方法

Module#class_eval方法(别名: module_eval)，会在一个已存在的类的上下文中执行一个块。使用该方法可以在不需要class关键字的前提下，打开类。

Module#class_eval与Object#instance_eval方法相比，后者instance_eval方法只能修改self，而class_eval方法可以同时修改self与当前类。

此外class_eval的另一个优势就是可以利用扁平作用域，规避class关键字的作用域门问题。

#### instance_eval 与 class_eval的选择

通常使用instance_eval方法打开非类的对象，而用class_eval方法打开类定义，然后用def定义方法。

-待续-
