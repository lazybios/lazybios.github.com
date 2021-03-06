---
layout: post
title: "编写清晰可维护正则的5个建议| Ruby"
date: "2016-08-08"
---

### 使用%r代替“/”
Ruby中使用`/`符号来包裹正则表达式，如`/^hello$/`，但往往会有这样的情况，即`\/`本身就是正则的一部分，比如常见的URL网址，尽管通过转义字符`\`转移后就不影响机器阅读了，但是正反斜杠混合到一起，却会严重的影响程序员之间代码的阅读。Python里有形如`r”\d”`这样的原生串概念，Ruby里也一样有类似的`%r{\d}`，下面是使用`%r`前后:

##### 使用%r前
```ruby
/\Ahttps?:\/\/(?:www\.)?github\.com\/([^\/]+)\/?\z/i
```
##### 使用%r后
```ruby
%r{\Ahttps?://(?:www\.)?github\.com/([^/]+)/?\z}i
```

注意结尾的`i`，即`%r`后仍然可以使用正则中类似`x, i, g`之类的选项。

### 使用x注释选项
在使用正则表达式的时候，往往很容易写出1行以上的负责表达式，特别是在解析日志的时候，对于超长正则，想要保证其可维护性，就需要为正则引入注释了，正则中的注释模式使用`x`选项开启，开启后其会忽略空格以及注释符(`#`)之后的内容。

##### 注释前
```ruby
doc.scan(/^ *(\#{1,6}) *(.+?) *\#* *$/)
```

##### 注释后
```ruby
doc.scan(/
  ^[ ]*     # 允许空格打头
  (\#{1,6}) # 捕获h1~h6 等级的标题
  [ ]*
  (.+?)     # 标题内容
  [ ]*
  \#*       # 允许装饰性的标题内容出现
  [ ]*$     # 行末允许有空格
/x)
```
上面正则目的是Markdown文本中的header标题，开启注释模式后，正则允许写成多行模式，同时会忽略空格和注释内容，但如果正则本身还有空格，那么需要用`[ ]`来转以表示。同样的如果出现了注释符号(`#`)，则需要`\#`来转义。

### 正则表达式插值
像字符串插值那样，正则表达式也支持通过插值的形式动态扩展表达式:

#####插值
```ruby
GITHUB_COM = %r{https?://(?:www\.)?github\.com}i
%r{\A#{GITHUB_COM}/([^/]+)/?\z}o
```
上面正则把之前的表达式，分成了两部分，通过`#{GITHUB_COM}`的方式插值组合而成，并且在结尾为正则开启了`o`选项，该选项表示缓存插值结果，只有第一次使用是通过`GITHUB_COM`运算，之后再用时就会直接使用上一次缓存好的结果。不过缓存虽好，不过对于动态内容使用起来要小心，否则很难定位问题。

### 避免捕获不必要的分组内容
正则除了验证匹配度之外，另一个有用的功能就是模式匹配捕获数据，在正则中括号的功效有两个，一个是对内容**分组**，另一个就是对指定模式进行**捕获**抽取。不过，尽管括号功能强大，但是也不应该什么内容都去捕获，因为捕获结果会占用内存，并且会拉低正则的执行效率。所以，我们应该只去关心那些对我们有用的内容。正则中可以通过`(?: )`的形式去声明跳过捕获。

```ruby
%r{\Ahttps?://(?:www\.)?github\.com/([^/]+)/?\z}i
```
我们只关心用户名，所以对于形如`www`这样的二级域名，可以直接丢弃掉。

### 为匹配项命名
类似解析Nginx或Apache日志的正则表达式，往往需要捕获的内容为数众多，默认情况下分组捕获括号是从左起第一位开始记为第1分组，第2分组…第n分组，当需要访问这些捕获后的分组内容时，就需要根据序号来依次访问(一般第0分组表示整体目标匹配)。如下代码:

```ruby
> r = %r{\A#{GITHUB_COM}/([^/]+)/([^/]+)/?\z}o
> m = r.match('http://github.com/AaronLasseigne/dotfiles')
=> #<MatchData "http://github.com/AaronLasseigne/dotfiles" 1:"AaronLasseigne" 2:"dotfiles">
> m[1]
=> "AaronLasseigne"
> m[2]
=> "dotfiles"
```

上面代码通过`m[1]`和`m[0]`分别访问Github项目的所有者和项目名。类似数组和哈希，后者通过有意义的键名管理访问起来自然会比前者要更清晰方便，但前者更具普适性。正则中也支持为捕获内容命名，可以通过命名像通过Key访问哈希一样访问捕获结果。如下，通过给分组命名，我们可以使用` m[:username]`和`m[:project]`来访问匹配项，是不是清晰多了呢？

```ruby
> r = %r{\A#{GITHUB_COM}/(?<username>[^/]+)/(?<project>[^/]+)/?\z}o
> m = r.match('http://github.com/AaronLasseigne/dotfiles')
=> #<MatchData "http://github.com/AaronLasseigne/dotfiles" username:"AaronLasseigne" project:"dotfiles">
> m[:username]
=> "AaronLasseigne"
> m[:project]
=> "dotfiles"
```

虽然是基于Ruby来说明的，但这些建议一样适用于其它语言，所涉及的功能和选项在其他语言中都能找到等价替代，所以无论用什么语言，合理适用上面5个建议，都能写出易于阅读和维护的正则代码。

-完-

### 参考引用
+ [http://t.cn/RtCIonk]()
