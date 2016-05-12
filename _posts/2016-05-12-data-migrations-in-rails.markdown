---
layout: post
title: "数据迁移的正确姿势 | Rails"
date: "2016-05-12"
---

不知道你有没有把数据迁移写入Migration文件的经历，相信无论是老鸟还是新手都这样干过吧。事实上，这样做并不是行不通，只不过这样的实践慢慢会给你引入一些不必要的麻烦。

一般认为`db/migrate`文件夹里的内容是关于你数据库Schema的演变过程，每个新的开发或线上环境都要通过这些Migration来构建可用的数据库。但如果这里装入了，负责细节的业务代码，比如一些历史遗留数据的迁移代码之类的，当一段时间后，数据库的结构变化了，但Migration没有跟着变化，渐渐的曾经的辅助代码，就成了垃圾代码，不仅不能帮忙构建环境，还会让`rake db:migrate`的执行过程异常中断，无形中增加了新环境的构建成本。

所以正确的做法应该是，**Migration只负责Schema相关的事宜，而不该过问数据的细节，具体的数据细节，全部交由`rake`任务来做**，并且这些`rake`任务也不是一成不变的，随着时间的推移它们也会废弃掉，但因为它们与系统的其它部分不想管，所以直接删掉即可。不过使用Rake做数据迁移也是有讲究的，具体如下:


#### Bad Rake Task
```ruby
# lib/tasks/temporary/users.rake
namespace :users do
  task :set_newsletter => :environment do
    User.all.each do |user|
      if user.confirmed?
        user.receive_newsletter = true
        user.save
      end
    end
  end
end
```
+ 任务会遍历所有用户，想想如果数据集很大会怎样
+ 通过ActiveRecord更新数据，会触发模型中的验证和创建回调方法
+ 通过`if`条件语句来判断是否需要更新数据
+ 不能直观的看出这个任务是干什么的，没有一个`desc`，所以也无法通过`rake -T`找到它

#### Good Rake Task
```ruby
# lib/tasks/temporary/users.rake
namespace :users do
  desc "Update confirmed users to receive newsletter"
  task set_newsletter: :environment do
    users = User.confirmed
    puts "Going to update #{users.count} users"

    ActiveRecord::Base.transaction do
      users.each do |user|
        user.mark_newsletter_received!
        print "."
      end
    end

    puts " All done now!"
  end
end
```
+ 通过`desc`我们可以清楚的知道任务的意图，并且它也会显示在`rake -T`中
+ 通过`scope`解决了`if`语句的问题
+ 引入了计数器，以及执行状态显示，能让我们了解到程序运行时的情况
+ 把数据的更改放到了事务中执行，可以语法因为数据异常，奔溃导致的不一致问题

最后要补充说明的一点是，有时候，可能直接用SQL语句更简单有效，特别是在数据集比较大的情况下，一条SQL能帮你省去不少无谓的循环！另外，记得上开发环境之前，最好预先检测一下Rake任务的有效性。

-完-

### 参考引用
+ [http://t.cn/RqdzbUS]()
