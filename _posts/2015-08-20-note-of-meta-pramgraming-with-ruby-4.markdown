---
layout: post
title: "《Ruby 元编程》读书笔记(四)"
date: "2015-08-20 14:17"
---j

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### Proc对象

Proc是由块转换来的对象。创建一个Proc共有四种方法,分别是:

#### 示例代码

{% highlight ruby linenos %}
# 法一
inc = Proc.new { | x | x + 1}
inc.call(2)  #=> 3

# 法二
inc = lambda {| x | x + 1 }
inc.call(2)  #=> 3

# 法三
inc = ->(x) { x + 1}
inc.call(2) #=> 3

# 法四
inc = proc {|x| x + 1 }
inc.call(2) #=> 3
{% endhighlight %}

除了上面的四种之外，还有一种通过&操作符的方式，将代码块与Proc对象进行转换。如果需要将某个代码块作为参数传递给方法，需要通过为这个参数添加&符号，并且其位置必须是在参数的最后一个

&符号的含义是： 这是一个Proc对象，我想把它当成代码块来使用。去掉&符号，将能再次得到一个Proc对象。

#### 示例代码
{% highlight ruby linenos %}
def my_method(&the_proc)
    the_proc
end

p = my_method {|name| “Hello, #{name} !”}
p.class   #=> Proc
p.call(“Bill”)   #=> “Hello,Bill”


def my_method(greeting)
    “#{greeting}, #{yield}!”
end

my_proc = proc { “Bill” }
my_method(“Hello”, &my_proc)
{% endhighlight %}

### Proc与Lambda对比

使用Lambda方法创建的Proc与其它方式创建的Proc是有一些差别的，用lambda方法创建的Proc称为lambda,而用其他方式创建的则称为proc。通过Proc#lambda?可以检测Proc是不是lambda。

二者之间主要的差异有以下两点:

+ Proc、Lambda的return含义不同; lambda中的return表示的仅仅是从lambda中返回。而proc中，return的行为则不同，其并不是从proc中返回，而是从定义proc的作用域中返回。即相当与在你的业务代码处返回。

#### 示例代码
{% highlight ruby linenos %}
def double(callable_object)
    p = Proc.new { return 10 }
    result = p.call   
    return result * 2 # 不可达的代码
end

double #=> 10
{% endhighlight %}

+ Proc、Lambda的return参数检查方式不同；Proc的参数检查要比Lambda参数检查要更宽松一些，如果传入Proc中的参数数量不匹配其不会发生报错，会自行进行一定的调整到期望参数的样子，但是对于lambda则不同，如果出现参数不匹配的情况，其往往会报ArgumentError异常，中断程序。
#### 示例代码
{% highlight ruby linenos %}
p = Proc.new { |a, b|  [a,b] }
p.call(1,2,3) #=> [1, 2]
p.call(1) #=> [1, nil]
{% endhighlight %}

### Proc与Lambda之间的选择

lambda更直观，更像是一个方法，参数要求更加严格，return也更像是方法定义中的return，往往Ruby程序员会将lambda作为第一选择。（PS: 这部分还真需要考证一下！）

-待续-
