---
layout: post
title: "《Ruby 元编程》读书笔记(十二)"
date: "2015-08-27 21:52"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### Kernal#eval方法

Kernal#eval方法与之前的BasicObject#instance_eval和Module#class_eval一样，都属于*eval家族，都可以赋予程序在运行中进行动态变化的能力。与后两者想比Kernal#eval更加直接，不需要代码块、直接就可以执行字符串代码(String of Code)。

PS:BasicObject#instance_eval也是可以执行字符串代码的。

#### 示例代码
{% highlight ruby linenos %}
array = [10, 20]
element = 30
eval(“array << element”)  #=> [10, 20, 30]
array.instance_eval "self << element"  #=> [10, 20, 30]
{% endhighlight %}

### Here文档(Here documents)

以<<打头，后面跟一个“结束序列标识”，之后就可以是正式的文档内容了，可以任意换行，直到遇到了独立出现的”结束序号标识”。
{% highlight ruby linenos %}
puts <<GROCERY_LIST
Grocery list
------------
1. Salad mix.
2. Strawberries.*
3. Cereal.
4. Milk.*

* Organic
GROCERY_LIST
{% endhighlight %}

上面代码的输出格式如下:
{% highlight ruby linenos %}
Grocery list
------------
1. Salad mix.
2. Strawberries.*
3. Cereal.
4. Milk.*

* Organic
=> nil
{% endhighlight %}

### 绑定对象

Binding是一个用对象标识的完整作用域(上下文环境)。可以通过创建Binding对象来捕获并带走当前的作用域。之后，通过eval方法在这个Binding对象所携带的作用域内执行代码。

使用Kernel#binding方法可以用来创建Binding对象

#### 示例代码
{% highlight ruby linenos %}
class MyClass
    def my_method
        @x = 1
        binding
    end
end

b = MyClass.new.my_method
eval “@x”, b #=> 1
{% endhighlight %}

上面代码，在MyClass类中定义了一个my_method方法来返回一个当前的绑定。最后将这个返回的绑定，作为参数传递给eval方法。这样“@x” 就可以在返回的绑定作用域中执行了。

关于绑定还有另外一个知识点，Ruby还提供了一个名为TOPLEVEL_BINDING的预定义常量，表示顶级作用域Binding对象。该常量可以在程序的任何位置访问到。言外之意，你可以在程序的任何位置，通过Kernal#eval方法在顶级作用域中执行代码。

#### 示例代码
{% highlight ruby linenos %}
class AnotherClass
    def my_method
        eval “self”, TOPLEVEL_BINDING
    end
end

AnotherClass.new.my_method  #=> main
{% endhighlight %}

#### 代码注入

Kernal#eval执行这样可以灵活执行字符串代码的特性，给编程带来了灵活性之外，也带来了潜在的风险，如果字符串代码来源于不可信的用户输入，如果不做安全检查，保不齐什么时候就会是一段破坏性的恶意代码。

面对这样的风险，可以选择规避eval的使用，换用其它相对安全的方式代替,例如**动态方法**和**动态派发**。此外，还可以通过在Ruby中通过修改$SAFE全局变量值，来控制程序的安全性级别，具体就是在你要执行**可信的**字符串代码前，将安全级别降低，可以使用Object#untaint方法，执行完之后在切换安全级别。这有点像操作系统中使用临界资源的步骤(请求锁，释放锁)

通过Object#tainted?方法可以判断一个对象是不是被污染了（是否来自一个不可信的输入源）。

-待续-
