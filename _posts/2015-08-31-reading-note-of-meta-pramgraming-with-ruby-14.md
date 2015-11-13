---
layout: post
title: "《Ruby 元编程》读书笔记(十四)"
date: "2015-08-31 21:43"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

前面已经对元编程的理论知识走了一遍，后面的内容侧重于实际操作和源码阅读，今天的Topic就是由书中第六章中的例子而来。

书中以迭代的形式，引导读者一步步实现一个名为attr_checked的类宏，正所谓上帝造人也需要7天，创造和生产这件事儿，过程和迭代都是少不了的，本文完全遵照guide而来，美中不足的地方是没有添加，测试用例，不过每个步骤都给出了一些简单的实例演示代码。

### 任务描述
写一个操作方法类似`attr_accessor`的`attr_checked`的类宏，该类宏用来对属性值做检验，使用方法如下:

{% highlight ruby linenos %}
class Person
  include CheckedAttributes

  attr_checked :age do |v|
    v >= 18
  end
end

me = Person.new
me.age = 39  #ok
me.age = 12  #抛出异常
{% endhighlight %}

### 实施计划
1. 使用eval方法编写一个名为`add_checked_attribute`的内核方法，为指定类添加经过简单校验的属性
2. 重构add_checked_attribute方法，去掉eval方法，改用其它手段实现
3. 添加代码块校验功能
4. 修改add_checked_attribute为要求的attr_checked,并使其对所有类都可用
5. 通过引入模块的方式，只对引入该功能模块的类添加attr_checked方法


#### Step 1
{% highlight ruby linenos %}
def add_checked_attribute(klass, attribute)
  eval "
    class #{klass}
      def #{attribute}=(value)
        raise 'Invalid attribute' unless value
        @#{attribute} = value
      end
      def #{attribute}()
        @#{attribute}
      end
    end
  "
end

add_checked_attribute(String, :my_attr)
t = "hello,kitty"

t.my_attr = 100
puts t.my_attr

t.my_attr = false
puts t.my_attr
{% endhighlight %}

这一步使用`eval`方法，用`class`和`def`关键词分别打开类，且定义了指定的属性的get和set方法，其中的set方法会简单的判断值是否为空(nil 或 false)，如果是则抛出`Invalid attribute`异常。

#### Setp 2
{% highlight ruby linenos %}
def add_checked_attribute(klass, attribute)
  klass.class_eval do
    define_method "#{attribute}=" do |value|
      raise "Invaild attribute" unless value
      instance_variable_set("@#{attribute}", value)
    end

    define_method attribute do
      instance_variable_get "@#{attribute}"
    end

  end
end

{% endhighlight %}

这一步更换掉了`eval`方法,同时也分别用`class_eval`和`define_method`方法替换了之前的`class`与`def`关键字，实例变量的设置和获取分别改用了`instance_variable_set`和`instance_variable_get`方法,使用上与第一步没有任何区别，只是一些内部实现的差异。

### Step 3
{% highlight ruby linenos %}
def add_checked_attribute(klass, attribute, &validation)
  klass.class_eval do
    define_method "#{attribute}=" do |value|
      raise "Invaild attribute" unless validation.call(value)
      instance_variable_set("@#{attribute}", value)
    end

    define_method attribute do
      instance_variable_get "@#{attribute}"
    end

  end
end

add_checked_attribute(String, :my_attr){|v| v >= 180 }
t = "hello,kitty"

t.my_attr = 100  #Invaild attribute (RuntimeError)
puts t.my_attr

t.my_attr = 200
puts t.my_attr  #200
{% endhighlight %}
没有什么奇特的，只是加了通过**代码块**验证，增加了校验的灵活性，不再仅仅局限于nil和false之间了。


### Step 4
{% highlight ruby linenos %}
class Class
  def attr_checked(attribute, &validation)
      define_method "#{attribute}=" do |value|
        raise "Invaild attribute" unless validation.call(value)
        instance_variable_set("@#{attribute}", value)
      end

      define_method attribute do
        instance_variable_get "@#{attribute}"
      end
  end
end

String.add_checked(:my_attr){|v| v >= 180 }
t = "hello,kitty"

t.my_attr = 100  #Invaild attribute (RuntimeError)
puts t.my_attr

t.my_attr = 200
puts t.my_attr  #200

{% endhighlight %}

这里我们把之前顶级作用域中方法名放到了`Class`中，由于所有对象都是`Class`的实例, 所以这里定义的实例方法，也能被Ruby中的其它所有类访问到，同时在class定义中,self就是当前类，所以也就省去了调用类这个参数和`class_eval`方法，并且我们把方法的名字也改成了`attr_checked`。

### Step 5
{% highlight ruby linenos %}
module CheckedAttributes
  def self.included(base)
    base.extend ClassMethods
  end
end

module ClassMethods
  def attr_checked(attribute, &validation)
      define_method "#{attribute}=" do |value|
        raise "Invaild attribute" unless validation.call(value)
        instance_variable_set("@#{attribute}", value)
      end

      define_method attribute do
        instance_variable_get "@#{attribute}"
      end
  end
end

class Person
  include CheckedAttributes

  attr_checked :age do |v|
    v >= 18
  end
end
{% endhighlight %}

最后一步通过**钩子方法**，在`CheckedAttributes`模块被引入后,对当前类通过被引入模块进行扩展，
从而使当前类支持引入后的方法调用，即这里的get与set方法组。

到此，我们已经得到了一个名为`attr_checked`，类似`attr_accessor`的类宏，通过它你可以对属性进行你想要的校验。

-待续-
