---
layout: post
title: "《Ruby 元编程》读书笔记(八)"
date: "2015-08-23 15:42"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 单件类

我们知道Ruby中对象的方法的查找顺序是: **先向右，再向上**，其含义就是先向右找到对象的类，先在类的实例方法中尝试查找，如果没有找到，再继续顺着**祖先链**找。

前面一篇中有介绍过**单件方法**，单件方法是指那些只针对某个对象有效的方法，那么如果为一个对象定义了单件方法，那么这个单件方法的查找顺序又应该是怎样的？

{% highlight ruby linenos %}
class MyClass
    def my_method; end
end

obj = MyClass.new

def obj.my_singleton_method; end
{% endhighlight %}

首先,**单件方法**不会在obj中，因为obj不是一个类，其次它也不在MyClass中，那样的话所有的MyClass都应该能共享调用这个方法，也就构不成单件类了。同理，单件方法也不能在**祖先链**的某个位置(类似superclass: Object)中。正确的位置是在**单件类**中，这个类其实就是我们在irb中向对象询问它的类时(obj.class)得到的那个类，不同的是这类与普通的类还是有稍稍不同的。也可以称其为**元类或本征类**。

### 打开单件类

Ruby提供了两种方法获取单件类的引用，一种是通过传统的关键词class配合特殊的语法

#### 法一
{% highlight ruby linenos %}
class << an_object
    # 自己的代码
end

obj = Object.new
singleton_class = class << obj
    self
end
singleton_class.class  # => Class
{% endhighlight %}

另一个方法是，通过Object#singleton_class方法来获得单件类的引用:

#### 法二
{% highlight ruby linenos %}
“abc”.singleton_class   # => #<Class: #<String:0xxxxxx>>
{% endhighlight %}

### 单件类的特性

+ 每个单件类只有一个实例（被称为单件类的原因），而且不能被继承
+ 单件类是一个对象的单件方法的存活所在

### 引入单件类后的方法查找

基于上面对单件类的基本认识，引入单件类后，Ruby的方法查找方式就不应该是先从其类(普通类)开始，而是应该先从对象的单件类中开始查找，如果在单件类中没有找到想要的方法，它才会开始沿着类(普通类)开始，再到祖先链上去找。这样从单件类之后开始，一切又回到了我们在没有引入单件类时候的次序。

通过下面这个代码可以自行验证一下
{% highlight ruby linenos %}
class C
    def a_method
        ‘C#a_method()’
    end
end

class D < C; end

obj = D.new
{% endhighlight %}
#打开单件类定义单件方法
{% highlight ruby linenos %}
class << obj
    def a_singleton_method
        ‘obj#a_singleton_method()’
    end
end

obj.singleton_class.superclass  #=> D
{% endhighlight %}

![Ruby元编程]({{site.IMG_PATH}}/metapragramming-8.jpg)

-待续-
