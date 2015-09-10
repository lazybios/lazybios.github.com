---
layout: post
title: "《Ruby 元编程》读书笔记(三)"
date: "2015-08-19 19:14"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 代码块

Ruby中的代码块这个Topic，之前的文章有介绍过，需要的同学可以翻一翻历史消息，这里做个简短的回顾与总结

代码块的定义方式有{}花括号与do...end关键字定义两种，单行用花括号，多行用do...end

代码块只有在方法调用的时候才可以定义，块会被直接传递给这个方法，判断某个方法调用中是否包含代码块，可以通过Kernel#block_given?

代码块不仅可以有自己的参数，也会有返回值，往往没有代码块中的最后一行执行结果会被作为返回值返回

代码块之所以可以执行，是因为其不仅包含代码，同时也涵盖一组相应绑定，即执行环境，也可以称之为上下文环境。代码块可以携带这个上下文环境，到任何一个代码块可以达到的地方。也就可以说，一个代码块是一个闭包，当定义一个代码块时，它会捕获当前环境中的绑定，并带它们四处流动。

###  作用域

Ruby中不具备嵌套作用域(即在内部作用域，可以看到外部作用域的)的特点，它的作用域是截然分开的，一旦进入一个新的作用域，原先的绑定会被替换为一组新的绑定。

程序会在三个地方关闭前一个作用域，同时打开一个新的作用域，它们是:

+ 类定义class
+ 模块定义 module
+ 方法定义 def

上面三个关键字，每个关键字对应一个作用域门(进入)，相应的end则对应离开这道门。

#### 扁平化作用域

从一个作用域进入另一个作用域的时候，局部变量会立即失效，为了让局部变量持续有效，可以通过规避关键字的方式，使用方法调用来代替作用域门，让一个作用域看到另一个作用域里的变量，从而达到目的。具体做法是，通过Class.new替代class，Module#define_method代替def,Module.new代替module。这种做法称为扁平作用域，表示两个作用域挤压到一起。

#####示例代码(Wrong)
{% highlight ruby linenos %}
my_var = “Success”
class MyClass
    puts my_var  #这里无法正确打印”Success”
    def my_method
        puts my_var  #这里无法正确打印”Success”
    end
end
{% endhighlight %}

##### 示例代码(Right)
{% highlight ruby linenos %}
my_var = “Success”
MyClass = Class.new do
    puts “#{my_var} in  the class definition”
    define_method :my_method do
        “#{my_var} in the method”
    end
end
{% endhighlight %}

#### 共享作用域

将一组方法定义到，某个变量的扁平作用域中，可以保证变量仅被有限的几个方法所共享。这种方式称为共享作用域

### instance_eval方法

这个BasicObject#instance_eval有点类似JS中的bind方法，不同的时，bind是将this传入到对象中，而instance_eval则是将代码块(上下文探针Context Probe)传入到指定的对象中，一个是传对象，一个是传执行体。通过这种方式就可以在instance_eval中的代码块里访问到调用者对象中的变量。

#### 示例代码
{% highlight ruby linenos %}
class MyClass
    def initialize
        @v = 1
    end
end
obj = MyClass.new

obj.instance_eval do
    self    #=> #<MyClass:0x33333 @v=1>
    @v      #=> 1  
end

v = 2
obj.instance_eval { @v = v }
obj.instance_eval { @v }   # => 2
{% endhighlight %}

此外，instance_eval方法还有一个双胞胎兄弟：instance_exec方法。相比前者后者更加灵活，允许对代码块传入参数。

#### 示例代码
{% highlight ruby linenos %}
class C
    def initialize
        @x = 1
    end
end
class D
    def twisted_method
        @y = 2
        #C.new.instance_eval { “@x: #{@x}, @y>: #{y}” }
        C.new.instance_exec(@y) { |y| “@x: #{@x}, @y: #{y}” }
    end
end
#D.new.twisted_method   # => “@x: 1, @y: ”
D.new.twisted_method   # => “@x: 1, @y: 2”
{% endhighlight %}

因为调用instance_eval后，将调用者作为了当前的self，所以作用域更换到了class C中，之前的作用域就不生效了。这时如果还想访问到之前@y变量，就需要通过参数打包上@y一起随instance_eval转义，但因为instance_eval不能携带参数，所以使用其同胞兄弟instance_exec方法。

### 洁净室

一个只为在其中执行块的对象，称为洁净室，其只是一个用来执行块的环境。可以使用BasicObject作为洁净室是个不错的选择，因为它是白板类，内部的变量和方法都比较有限，不会引起冲突。

-待续-
