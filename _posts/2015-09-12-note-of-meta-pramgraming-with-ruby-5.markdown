---
layout: post
title: "《Ruby 元编程》读书笔记(五)"
date: "2015-08-21 18:33"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 可调用对象

+ 代码块(不是真正的对象，但是它们是”可调用的”):在定义它们的作用域中执行
+ proc: Proc类的对象与代码块一样，也在定义自身的作用域中执行
+ lambda: 同样是Proc类的对象，但跟普通的proc有细微差别。与代码块与proc一样都是闭包，也在定义自身的作用域中执行
+ 方法: 绑定于一个对象，在 所绑定对象的作用域中执行。他们也可以与这个作用域解除绑定，然后再重新绑定到另一个对象的作用域中
前三个在之前的笔记中都有详细介绍，今天来详细看下最后一个Method对象部分

#### Method 对象

通过Kernel#method方法，可以获得一个用Method对象表示的方法，在之后可以用Method#call方法对其进行调用。同样也可以用Kernel#singleton_method方法把单件方法名转换为Method对象

##### 示例代码
```ruby
class MyClass
    def initialize(value)
        @x = value
    end
    def my_method
        @x
    end
end

obj = MyClass.new(1)
m = obj.method :my_method
m.call   #=> 1
```

#### 自由方法

与普通方法类似，不同的是它会从最初定义它的类或模块中脱离出来(即脱离之前的作用域)，通过Method#unbind方法，可以将一个方法变为自由方法。同时，你也可以通过调用Module#instance_method获得一个自由方法.

###### 示例代码
```ruby
module MyClass
    def my_method
        42
    end
end

unbound = MyModule.instance_method(:my_method)
unbound.class  #=> UnboundMethod
```

上面代码中的UnboundMethod对象，不能够直接用来调用，不过可以将一个UnboundMethod方法绑定到一个对象上，使其再次成为一个Method对象，然后再被调用执行。

上面的绑定过程可以通过UnboundMethod#bind方法把UnboundMethod对象绑定到一个对象上，从某个类中分离出来的UnboundMethod对象只能绑定在该类及其子类对象上，不过从模块中分离出来的UnboundMethod对象在Ruby2.0后就不在有此限制了。

此外还可以将UnboundMethod对象传递给Module#define_method方法，从而实现绑定。

```ruby
String.send :define_method, :another_method, unbound
“abc”.anther_method  #=> 42
```

PS: 上面代码请与前一小节示例代码，结合起来看！

-待续-
