---
layout: post
title: "《Ruby 元编程》读书笔记(十一)"
date: "2015-08-26 14:26"
---

![Ruby元编程]({{site.IMG_PATH}}/metaprogramming-1.jpg)

### 方法包装器（Method Wrapper）

方法包装器一般适合于，处理一些你不能直接触碰到的方法，或者需要给某个方法进行一些预处理工作，这个概念有点类似与Python中的装饰器

#### 方法别名

Ruby中使用Module#alias_method方法和alias关键字为方法取别名。

##### 示例代码
```ruby
class MyClass
    def my_method
         “my_method()”
    end
    alias_method :m, :my_method
end

obj = MyClass.new
obj.my_method    #=> “my_method()”
obj.m   #=> “my_method()”
```

需要注意的是，在顶级作用域中（main）中只能使用alias关键字来命名别名，因为在那里调用不到Module#alias_method方法

此外这里，还需要指出一点关于**方法重定义**的理解误区，一般我们认为方法一旦被重定义了，就找不回原来的方法了，其实在Ruby中的重定义只是，对方法名与方法实体绑定的一种重新绑定，如果被重新定义的方法体还有一个其它别名，那么一样还是可以访问到老方法的。

有了上面这个理解，我们来看**环绕别名**这个概念。

#### 环绕别名

经过下面三个步骤可以实现**环绕别名**

1. 给方法定义一个别名
2. 重定义这个方法
3. 在新的方法中调用老的方法

上面三步后，即可以保留老方法(第一步)，同时又为原方法增加了新功能。但其本质上没有改变调用之间的函数名称依赖，是一种名副其实的“新瓶装老酒”的取巧办法。

不过**环绕别名**也有缺点，就是它污染了类的命名空间，添加了额外的方法名。不过这一点其实可以通过Ruby中的private关键字把旧方法限定为私有方法。(Ruby中的public和private实际上针对的是方法名，而非方法本身，言外之意为private方法取个别名，也可以把私有方法暴露出来)

最后要注意的是不要尝试load两次**环绕别名**的实现代码，否则得到的执行结果就不是你预期中的了，不信你可以试着连续走两遍上面的3个步骤。

#### 细化

前面提到的细化，也可以作为一种方法包装器，在细化的方法中调用super方法，可以调用会细化之前的方法。此外细化相比起**环绕别名**，细化的作用范围是文件末尾，而环绕别名则是作用在全局。

```ruby  
module StringRefinement
  refine String do
    def length
      super > 5 ? 'long' : 'short'
    end
  end
end

using StringRefinement

puts "War and Peace".length  #=> “long”
```

#### 下包含包装器 (Module#prepend)

在介绍**祖先链**的部分有涉及过include与prepend两个方法，前者是将模块插入到当前类的上方，后者是插入到下方，而下方的位置，正好是方法查找时优先查找的位置，利用这一优势，可以覆写当前类的同名方法，同时通过super关键字还可以调用到该类中的原始方法

```ruby
module ExplicitString
    def length
        super > 5 ? ‘long’ : ‘short’
    end
end

String.class_eval do
    prepend ExplicitString
end

puts "War and Peace".length  #=> “long”
```

-待续-
