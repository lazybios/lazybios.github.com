---
layout: post
title: "一个关于freeze的小问题 | Ruby"
date: "2017-01-11"
---

来分享一个StackoverFlow上关于`freeze`的小问题。Ruby里的`freeze`方法可以将一个Ruby对象冻结起来防止其被意外更改。但下面这段代码居然没有报错，是不是很奇怪呢？

```ruby
a = "Test"
a.freeze
a += "this string"
puts a

Test this string
[Finished in 0.0s]
```

行为上看起来有些吊诡，但实际上问题并没有出在`freeze`上，`freeze`所限制的是一个对象，而这里确实为一个变量重新赋值，下面两句其实是等价的:

```ruby
a += "this string"
```

```ruby
a = a + "this string"
```

也就是说`"Test"`对象并没有被修改，其仍然在内存中，只不过现在成了一个无法被访问等待回收的垃圾对象。这一点可以通过`a.object_id`观察到。

当你真正要修改freeze对象时，它依然会抛出一个运行时错误，像下面这样:

```ruby
a << "this string"
RuntimeError: can't modify frozen String
```

-完-

### 参考引用
+ [http://t.cn/RMa8hiP]()