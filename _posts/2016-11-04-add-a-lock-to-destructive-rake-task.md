---
layout: post
title: "给破坏性的Rake任务加把锁 | Rails"
date: "2016-11-04"
---

![rails_logo]({{site.IMG_PATH}}/rails_logo.png)

有时候我们需要借助Rake去执行一些批处理的工作，这些工作中相当大一部分是有破坏性的，在开发环境中执行无伤大雅，但是如果错误的在线上环境被执行了，会带来不小的麻烦。那么有没有什么办法可以避免这样的错误发生呢？今天分享这个Tricks就可以用来帮忙杜绝这样的问题。

```ruby
# lib/tasks/skip_prod.rake
desc 'Raises exception if used in production'
task skip_prod: [:environment] do
  raise 'You cannot run this in production' if Rails.env.production?
end
```
`rake environment`的目的是帮助加载Rails运行环境，有了它类似`Rails.env.production?`这样的方法就可以在Rake任务中被调用到。OK，一旦你定义好这个Rake任务，你就可以把它加到别的任务执行依赖之中，像下面那样任务执行前会依次执行依赖里的任务。

```ruby
# lib/tasks/db.rake
namespace :db do
  desc 'Drop, create, migrate then seed the development database'
  task reseed: [ 'skip_prod', 'db:drop', 'db:migrate', 'db:seed' ]
end
```
嗯，这样看起来自定义的任务如果都加上这个前置的检查应该问题不大，但是对于那些Rails自带的`db:drop`、`db:reset`呢，该如何给它们添加前置检查？其实这里要说的是一个叫`Rake::Task#enhence`的方法，它可以把像一个已经存在的Task中添加前置条件或动作，并返回任务本身，效果跟上面自定义Task一样。

```ruby
# lib/tasks/db.rake
['db:drop', 'db:reset', 'db:seed'].each do |t|
  Rake::Task[t].enhance ['skip_prod']
end
```

-完-


### 参考引用
+ [http://t.cn/RVFMM1D](http://t.cn/RVFMM1D)
