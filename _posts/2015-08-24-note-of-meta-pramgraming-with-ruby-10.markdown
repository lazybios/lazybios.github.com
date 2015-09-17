---
layout: post
title: "note-of-meta-pramgraming-with-ruby-10"
date: "2015-08-24 12:31"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 模块与单件类

当一个类包含一个模块时，他获得的是该模块的实例方法，而不是类方法。类方法存在与模块的单件类中，没有被类获得。

### 类扩展

类扩展通过向类的单件类中添加模块来定义类方法。

{% highlight ruby linenos %}
module MyModule
    def my_method; ‘hello’; end
end

class MyClass
    class < self
        include MyModule
    end
end

MyClass.my_method
{% endhighlight %}

上面代码展示了具体**类扩展**的实现方式，将一个MyModule模块引入到MyClass类的单件类中，因为my_method方法是MyClass的单件类的一个实例方法，这样，my_method方法也是MyClass的一个类方法。

### 对象扩展

**类方法**是**单件方法**的特例，因此可以把类扩展这种技巧应用到**任意对象**上，这种技巧即为**对象扩展**

{% highlight ruby linenos %}
# 法一: 打开单件类来扩展
module MyModule
    def my_method; ‘hello’; end
end

obj = Object.new
class << obj
    include MyModule
end

obj.my_method   # => “hello”
obj.singleton_methods   # => [:my_method]
# 法二：Object#extend方法
module MyModule
    def my_method; ‘hello’; end
end

obj = Object.new
#对象扩展
obj.extend MyModule
obj.my_method   # => “hello”  
#类扩展
class MyClass
    extend MyModule
end

MyClass.my_method  # => “hello”
{% endhighlight %}

Object#extend是在接受者的单件类中包含模块的快键方式。

-待续-
