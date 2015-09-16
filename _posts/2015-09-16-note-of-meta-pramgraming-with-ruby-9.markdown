---
layout: post
title: "《Ruby 元编程》读书笔记(九)"
date: "2015-08-24 19:33"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### Ruby对象模型的7条规则

1. 只有一个对象 —— 要么是普通对象，要么是模块
2. 只有一个模块 —— 可以是一个普通模块、一个类或者一个单件类
3. 只有一个方法，它存在于一个模块中 —— 通常是一个类中
4. 每个对象（包括类）都有自己的“真正的类”—— 要么是一个普通类，要么是一个单件类
5. 除了BasicObject类没有超类外，每个类有且只有一个祖先(superclass) —— 要么是一个类，要么是一个模块。这意味着任何类只有一条向上的，直到BasicObject的祖先链
6. 一个**对象**的单件类的超类是这个对象的类；一个**类**的单件类的超类是这个类的超类的单件类。(这条建议结合后面的附图来看！)
7. 调用一个方法时，Ruby先向右迈出一步进入接收者真正的类(单件类)，然后向上进入祖先链。

![Ruby元编程]({{site.IMG_PATH}}/ruby-meta-programming-9.jpeg)

### 类方法

类方法其实质是生活在该类的单件类中的单件方法。其定义方法有三种，分别是:

{% highlight ruby linenos %}
# 法一
def MyClass.a_class_method; end


# 法二
class MyClass
    def self.anther_class_method; end
end


# 法三*
class MyClass
    class << self
        def yet_another_class_method; end
    end
end
{% endhighlight %

其中第三种方法道出了，类方法的实质，特别记忆一下！

-待续-
