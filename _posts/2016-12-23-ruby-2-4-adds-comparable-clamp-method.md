---
layout: post
title: "Comparable#clamp方法 | Ruby 2.4"
date: "2016-12-23"
---

Ruby 2.4为`Compareable`模块引入了一个叫`clamp`的方法，这个方法可以用来限定一个值让其始终介于某个区间范围，实现效果类似下面代码:

```ruby
# 实现一
if x <=> min < 0, x = min; 
if x <=> max > 0 , x = max
else x

# 实现二
x = [a, [x, b].min].max

# 实现三
x = [a,x,b].sort[1]
```

### Clamping numbers
确保某个数值类型处于min、max之间。

```ruby
10.clamp(5, 20)
=> 10

10.clamp(15, 20)
=> 15

10.clamp(0, 5)
=> 5
```

### Clamping strings
语义化控制字符类值处于某个限定区间。

```ruby
"e".clamp("a", "s")
=> "e"

"e".clamp("f", "s")
=> "f"

"e".clamp("a", "c")
=> "c"

"this".clamp("thief", "thin")
=> "thin"
```

-完-

### 参考引用
+ [http://t.cn/RI0cL0Y](http://t.cn/RI0cL0Y)
+ [http://t.cn/RI0MSxl](http://t.cn/RI0MSxl)
