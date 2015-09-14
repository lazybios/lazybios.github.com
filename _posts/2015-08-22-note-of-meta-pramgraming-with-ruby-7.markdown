---
layout: post
title: "《Ruby 元编程》读书笔记 (七)"
date: "2015-08-22 11:17"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 单件方法

Ruby允许给单个对象增加方法,这种只针对单个对象生效的方法，称为单件方法

##### 示例代码
{% highlight ruby linenos %}
str = “just a regular string”

def str.title?
    self.upcase == self
end

str.title?  # => false
str.methods.grep(/title?/) # => [:title?]
str.singleton_methods   #=> [:title?]

str.class # => String
String.title?  #=>  NoMethodError
{% endhighlight %}

另外，除了上面使用的定义方法，还可以通过Object#define_singleton_method方法来定义单件方法

#### 单件方法与类方法

前面的笔记中有说道在Ruby中类也是对象，而类名只是常量，所以，在类上调用方法其实跟在对象上调用方法一样:

类方法的实质是: 它是一个类的单件方法，实际上，如果比较单件方法的定义和类方法的定义，会发现其实二者是一样的

{% highlight ruby linenos %}
def obj.a_singleton_method; end
def MyClass.another_class_method; end
{% endhighlight %}

二者均使用了def关键词做定义

{% highlight ruby linenos %}
def object.method
    #方法主体
end
{% endhighlight %}

上面的object可以是*对象的引用、常量类名或者self。

### 类宏attr_accessor

Ruby对象没有属性，如果希望得到一些像属性的东西，需要分别定义一个读方法和写方法（也就是java、objc中的set和get方法)，最直接的可以这样:

##### 示例代码
{% highlight ruby linenos %}
class MyClass
    def my_attribute=(value)
        @my_attribute =value    
    end
    def my_attribute
        @my_attribute
    end
end

obj = MyClass.new
obj.my_attribute = ‘x’
obj.my_attribute    #=> ‘x’
{% endhighlight %}

但是上面这种写法，如果属性众多的话就会存在Repeat Yourself的地方，这时就可以用到下面三个类宏:

+ Module#attr_reader 生成一个读方法
+ Module#attr_writer 生成一个写方法
+ Module#attr_accessor 同时生成读方法和写方法

##### 示例代码
{% highlight ruby linenos %}
class MyClass
    attr_accessor :my_attribue
end
{% endhighlight %}

这样是不是就简洁多了呢? 当然，使用方法(读与写)跟上面的实现是一致的。

-待续-
