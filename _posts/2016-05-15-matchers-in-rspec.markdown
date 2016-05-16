---
layout: post
title: "Rspec中的Matchers | Rails"
date: "2016-05-15"
---

最近刚刚给一个之前的旧项目补全了测试，而且可预见的未来还要给另一个项目在测试上填坑，所以这段时间Rspec看的要稍微多一点，我自己之前是那种关注`make it work`多一些的选手，因为一般项目并不大的缘故，所以也迟迟看不到危害在哪里，不过随着时间推移,项目变大，没有测试导致的问题也开始日益曝露了，其中首当其冲的就是不敢做重构，甚至在修bug的时候，也会出现搞定bug，但又引入了新坑(如下图)，今天这篇就来分享一些Rspec中常用的内置Matchers用法。

![]()
![when fixed code]({{site.IMG_PATH}}/giphy.gif)

### 内置Matchers介绍
Rspec内置Matchers可以被用在`expect`和`should`语法中，用法如下：

```
expect(result).to   eq(3)
expect(list).not_to be_empty

actual.should     eq(expected)
actual.should_not match(/expression/)
```
上面`eq`，`be_empty`以及`match`就是内置`matcher`，根据使用场景它们有许多不同的分类，具体如下范例:

### 比较
```
expect(actual).to be >  expected
expect(actual).to be >= expected
expect(actual).to be <= expected
expect(actual).to be <  expected
expect(actual).to be_between(minimum, maximum).inclusive
expect(actual).to be_between(minimum, maximum).exclusive
expect(actual).to match(/expression/)
expect(actual).to be_within(delta).of(expected)
expect(actual).to start_with expected
expect(actual).to end_with expected
```

### 类相关
```
expect(actual).to be_instance_of(expected)
expect(actual).to be_kind_of(expected)
expect(actual).to respond_to(expected)
```

### 布尔相关
```
expect(actual).to be_truthy    # passes if actual is truthy (not nil or false)
expect(actual).to be true
expect(actual).to be_falsey    # passes if actual is falsy (nil or false)
expect(actual).to be false
expect(actual).to be_nil
expect(actual).to exist        # passes if actual.exist? and/or actual.exists? are truthy
expect(actual).to exist(*args)
```

### 异常相关
```
expect { ... }.to raise_error
expect { ... }.to raise_error(ErrorClass)
expect { ... }.to raise_error("message")
expect { ... }.to raise_error(ErrorClass, "message")

expect { ... }.to throw_symbol
expect { ... }.to throw_symbol(:symbol)
expect { ... }.to throw_symbol(:symbol, 'value')
```

### 断言相关
```
expect(actual).to be_xxx
expect(actual).to have_xxx(:arg)

expect([]).to      be_empty
expect(:a => 1).to have_key(:a)
```

### 集合相关
```
expect(actual).to include(expected)
expect(array).to match_array(expected_array)
expect(array).to contain_exactly(individual, elements)

expect([1, 2, 3]).to     include(1, 2)
expect(:a => 'b').to     include(:a => 'b')
expect("this string").to include("is str")
expect([1, 2, 3]).to     contain_exactly(2, 1, 3)
expect([1, 2, 3]).to     match_array([3, 2, 1])
```

### 检查操作前后变化
```
expect { object.action }.to change(object, :value).from(old).to(new)
expect { object.action }.to change(object, :value).by(delta)
expect { object.action }.to change(object, :value).by_at_least(minimum_delta)
expect { object.action }.to change(object, :value).by_at_most(maximum_delta)
```
这个比较有用，比如通过`reload`比较操作前后，数据库中内容变化，如下例，不过要特别注意，这里`expect`后用的是块，花括号，而不是直接的参数。

```
expect { a += 1 }.to change { a }.by(1)
expect { a += 3 }.to change { a }.from(2)
expect { a += 3 }.to change { a }.by_at_least(2
```

### 块相关(yield)
```
expect { |b| object.action(&b) }.to yield_control
expect { |b| object.action(&b) }.to yield_with_no_args
expect { |b| object.action(&b) }.to yield_with_args
expect { |b| object.action(&b) }.to yield_successive_args(*args)
```

Ok，就是这些，这里罗列的是 Rspec 3.4的用法，严格意义上应该算是篇译文，所以有需要的还是建议直接穿越到官网找对应的matcher研究，如果觉得有用，那就快去给你的程序加个测试吧。

-完-

### 参考引用
+ [http://t.cn/RqDxy44]()
