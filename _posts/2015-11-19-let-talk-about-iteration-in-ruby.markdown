---
layout: post
title: "Ruby: 聊一聊关于集合遍历的各种方法"
date: "2015-11-19 21:54"
---

之前写过一篇关于迭代的，不过只介绍到了each和map(collect)之间的区别，今天看到一篇不错的关于集合遍历方法的文章，整理了一下拿来与大家分享，有需要看原文的同学可以从参考引用处穿越。

### each
each的作用基本取代了ruby中的关键词`for`，当你需要迭代一个集合的时候，应该首先想到它，无论是从语义上还是便捷性上，each都远胜于for

```ruby
['a', 'b', 'c'].each do |e|
  #do something...
end
```

### map
map的作用于each大体相同，最大同是map会将其处理后的结果集合作为返回值返回，这样就能省去了那些不必要的中间变量

```ruby
[1, 2, 3].map { |e| e * 2 } # returns [2, 4, 6]

['a', 'b', 'c'].map { |e| e.upcase } # returns ['A', 'B', 'C']

['a', 'b', 'c'].map(&:upcase)
```

### select  
可以把它当做一个正向的过滤器(filter)，选择返回select迭代中计算结果为true的元素

```ruby
[1, 2, 3, 4].select { |e| e % 2 == 0 } # returns [2, 4]
```

### reject
与`select`相反，可以看作是反向过滤器(filter)，选择返回reject中迭代计算结果为false的元素

```
[1, 2, 3, 4].reject { |e| e % 2 == 0 } # returns [1, 3]
```

### partition
根据传入`partition`代码块中的条件，把集合结果分为两部分: 满足条件(true)和不满足条件(false),其实就相当于同时应用了`select`与`reject`过滤器

```ruby
[2, 3, 4, 5].partition { |e| e.even? } # returns [[2, 4], [3, 5]]
```

### find
`find`可以用来查找集合中满足条件的**第一个**值，并会作为结果返回。如果你要找的是集合中的**所有同类值**，那么估计find就帮不到你了。

```ruby
[4, 6, 8, 13].find { |e| e > 7 } # Returns 8 (the first found element)
```

### reduce
昨天才发文解释了这个函数，这里直接贴代码了，不明白的可以回头看[Ruby: 一次搞懂Reduce，Inject方法](http://lazybios.com/2015/11/once-understand-inject-reduce/),这货比较强大，可以用来实现这里讲到的其它几个迭代方法，例如`map`,`select`,`find`等，改天可以专门写一篇文章来介绍它。

```ruby
(1..8).reduce(:+)
=> 36

(1..8).reduce { |sum, num| sum += num }
=> 36

(1..8).reduce(0) { |sum, num| sum += num }
=> 36

```

### all?
通过迭代来判断某个集合中的所有元素是否满足`all?`代码块中所给条件，只有当所有元素都满足才会返回true,否则返回false

```ruby
[2, 4, 6].all? { |e| e.even? } # returns true
```

### any?
集合中只要有一个满足条件的就返回true

```ruby
[3, 8, 42].any? { |e| e > 10 } # returns true
[3, 4].any? # returns true
[].any? # returns false
[nil].any? # returns false
[false].any? # returns false
```

### times
times在这里做次数的意思，表示执行代码块多少次

```ruby
3.times { puts 'Hello world!' } # prints 'Hello world!' 3 times

3.times { |i| puts i } # prints '0', '1' and '2'
```

### each_with_index
each_with_index可以在遍历集合的同时，给出你元素所在的下标位置。

```ruby
['a', 'b', 'c'].each_with_index do |e, i|
  # do stuff
end

['a', 'b', 'c'].map.with_index do |e, i|
  # do stuff
end
```

### while、for、loop、until
明显是用来凑内容的，一般的Ruby教程都会介绍这个关键字的用法,这里只贴一遍代码，不做过多介绍了:

```ruby
# while
x = 100
while x > 0
  # do stuff
end

# for
for counter in 1..5
  puts "iteration #{counter}"
end

# loop
loop do
 # do stuff.
 # You can still exit the loop with break.
end

# until
finished = false
until finished
  # do stuff
end

```

-完-

### 参考引用
+ [http://dwz.cn/2cIm95]()
