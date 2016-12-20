---
layout: post
title: "Numeric的两个新方法 | Ruby 2.4"
date: "2016-12-20"
---

Ruby中有三个特殊值，正无穷(Infinity)、负无穷(-Infinity)以及非数值(NaN)三种，Ruby 2.4以前，只有`Float`与`BigDecimal`这两个类才有可以判断正负无穷的方法:`infinite?`与`finite?`。

```ruby
puts 1.0 / 0    # Infinity
puts -1.0 / 0   # -Infinity
puts 0.0 / 0.0  # NaN

```

尽管对于`Integer`这类数值来说，判断无穷与否都只会有一种结果，但为了保证数值类在行为上的一致性，Ruby2.4中给Numberic也加了这两个方法。

### Ruby 2.3

```ruby
#infinite?

5.0.infinite?
=> nil

Float::INFINITY.infinite?
=> 1

5.infinite?
NoMethodError: undefined method `infinite?' for 5:Fixnum
```

```ruby
#finite?

5.0.finite?
=> true

5.finite?
NoMethodError: undefined method `finite?' for 5:Fixnum
```

### Ruby 2.4

```ruby
#infinite?

5.0.infinite?
=> nil

Float::INFINITY.infinite?
=> 1

5.infinite?
=> nil
```

```ruby
#finite?

5.0.finite?
=> true

5.finite?
=> true
```

这种设计方法挺有意思的，为了强调整体的一致性，会适当的对一些部件做些增补删减以求平衡对称。

-完-

### 参考引用
+ [http://t.cn/RIagftd]()
