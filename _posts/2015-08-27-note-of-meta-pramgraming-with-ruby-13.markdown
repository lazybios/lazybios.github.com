---
layout: post
title: "《Ruby 元编程》读书笔记(十三)"
date: "2015-08-27 23:25"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 钩子方法

钩子方法有些类似事件驱动装置，可以在特定的事件发生后执行特定的回调函数，这个回调函数就是**钩子方法**(更形象的描述: 钩子方法可以像钩子一样，勾住一个特定的事件。)，在Rails中before\after函数就是最常见的钩子方法。

Class#inherited方法也是这样一个钩子方法，当一个类被继承时，Ruby会调用该方法。默认情况下，Class#inherited什么都不做，但是通过继承，我们可以拦截该事件，对感兴趣的继承事件作出回应。
```ruby
class String
    def self.inherited(subclass)
        puts “#{self} was inherited by #{subclass}”
    end
end
class MyString < String; end
#输出
String was inherited by MyString
```

通过使用钩子方法，可以让我们在Ruby的类或模块的生命周期中进行干预，可以极大的提高编程的灵活性。

这些生命周期相关的钩子方法还有下面这些:

#### 类与模块相关

+ Class#inherited
+ Module#include
+ Module#prepended
+ Module#extend_object
+ Module#method_added
+ Module#method_removed
+ Module#method_undefined

#### 单件类相关

+ BasicObject#singleton_method_added
+ BasicObject#singleton_method_removed
+ BasicObject#singleton_method_undefined

##### 示例代码
```ruby
module M1
    def self.included(othermod)
        puts “M1 was included into #{othermod}”
    end
end

module M2
    def self.prepended(othermod)
        puts “M2 was prepended to #{othermod}”
    end
end

class C
    include M1
    include M2
end

# 输出
M1 was included into C
M2 was prepended to C

module M
    def self.method_added(method)
        puts “New method: M##{method}”
    end

    def my_method; end
end

# 输出
New method: M#my_method
```

除了上面列出来的一些方法外，也可以通过重写父类的某个方法，进行一些过滤操作后，再通过调用super方法完成原函数的功能，从而实现类似钩子方法的功效，如出一辙，**环绕别名**也可以作为一种钩子方法的替代实现。

-待续-
