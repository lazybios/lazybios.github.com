---
layout: post
title: "没有副作用的Regexp#match?方法 | Ruby2.4"
date: "2016-11-14"
---


Ruby处理正则场景时，有下面几类方法可以用来验证正则匹配的结果。

#### Regexp#===
```ruby
/stat/ === "case statements"
#=> true
$~
#=> #<MatchData "stat">
```
返回匹配验证的布尔结果的同时会将匹配结果保存到`$~`全局变量中。

#### Regexp#=~
```ruby
/stat/ =~ "case statements"
#=> 5
$~
#=> #<MatchData "stat">
```

该方法匹配成功时会返回匹配结果所在位置，失败则返回`nil`。与`Regexp#===`一样会将匹配结果保存到`$~`全局变量中。

#### Regexp#match
```ruby
/stat/.match("case statements")
#=> #<MatchData "stat">
$~
#=> #<MatchData "stat">
```

匹配成功会返回匹配到的`MatchData`对象，并且与前面两个一样会将匹配结果计入`$~`中。

### 2.4新方法Regexp#match?
```ruby
/case/.match?("case statements")
#=> true
```

这个`Regexp#match?`正如函数名显示的那样，它仅返回一个匹配结果的布尔值，除此之外没有任何的副作用。

你可能要问，那么有这么多正则验证的选择了，有必要非得用这个`match?`方法吗？那要看你的使用场景了，不过因为不需要创建分配`MatchData`对象和修改`$~`的值，所以`Regexp#match?`在性能上肯定要略胜其它方法一筹。

不过话说会来，既然都选择了Ruby，那么效率问题肯定也不是你的首要瓶颈，所以这个方法其实也仅仅是个锦上添花的功能l。

```ruby
require 'benchmark/ips'

Benchmark.ips do |bench|

  EMAIL_ADDR = 'disposable.style.email.with+symbol@example.com'
  EMAIL_REGEXP_DEVISE = /\A[^@\s]+@([^@\s]+\.)+[^@\W]+\z/

  bench.report('Regexp#===') do
    EMAIL_REGEXP_DEVISE === EMAIL_ADDR
  end

  bench.report('Regexp#=~') do
    EMAIL_REGEXP_DEVISE =~ EMAIL_ADDR
  end

  bench.report('Regexp#match') do
    EMAIL_REGEXP_DEVISE.match(EMAIL_ADDR)
  end

  bench.report('Regexp#match?') do
    EMAIL_REGEXP_DEVISE.match?(EMAIL_ADDR)
  end

  bench.compare!
end

#=> Warming up --------------------------------------
#=>          Regexp#===   103.876k i/100ms
#=>           Regexp#=~   105.843k i/100ms
#=>        Regexp#match    58.980k i/100ms
#=>       Regexp#match?   107.287k i/100ms
#=> Calculating -------------------------------------
#=>          Regexp#===      1.335M (± 9.5%) i/s -      6.648M in   5.038568s
#=>           Regexp#=~      1.369M (± 6.7%) i/s -      6.880M in   5.049481s
#=>        Regexp#match    709.152k (± 5.4%) i/s -      3.539M in   5.005514s
#=>       Regexp#match?      1.543M (± 4.6%) i/s -      7.725M in   5.018696s
#=>
#=> Comparison:
#=>       Regexp#match?:  1542589.9 i/s
#=>           Regexp#=~:  1369421.3 i/s - 1.13x  slower
#=>          Regexp#===:  1335450.3 i/s - 1.16x  slower
#=>        Regexp#match:   709151.7 i/s - 2.18x  slower

```


-完-

### 参考引用
+ [http://t.cn/RfqeBqg](http://t.cn/RfqeBqg)