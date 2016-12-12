---
layout: post
title: "几个Rakefile的小技巧"
date: "2016-12-12"
---

​除了Rake任务本身的标准执行方式外，其实在编写Rakefile时还有一些小技巧能够用来改善Rake的常规执行，从而优化你的工作流。

### 链式调用
不同的Rake任务之间可以互相调用，即使它们在不同的`namespace`中。如下，执行`rake db:db:environment`时会同时触发执行`db:environment`和`environment`两个任务。

```ruby
namespace :db do
  task :environment => :environment do
    puts "db:environment"
  end
  task :migrate => :environment do
    puts "db:migrate"
  end
end

task :environment do
  puts "environment"
end
```


### 记录任务执行状态
对于那些只在后台跑的Rake任务，因为没有显示信息，所以调试起来总是特别的费劲儿。这时你可以试一试下面这招，执行`raek verbose deploy`

```ruby
LOGGER = Logger.new

task :verbose do
  LOGGER.level = Logger::DEBUG
end

task :deploy do
  LOGGER.debug(:environment){ENV}
  # ...
end
```

### Stateful Pipelines
借助实例变量，实现管道流的功能:

```ruby
mespace :dump do
  task :users do
    @records = Users.all
  end
  task :posts do
    @records = Posts.all
  end
  task :updated_recently do
    @records = @records.where("updated_at > ?", 6.months.ago)
  end
  task :as_json do
    $stdout.write(@records.as_json)
  end
  task :as_xml do
    $stdout.write(@records.as_xml)
  end
end
```

### 定义多个同名任务
Rake任务允许定义多个同名任务。如下，执行`rake deploy`会同时触发两个`deploy`任务，通过这种方式可以将一个大任务切分成多个子任务重新组织和简化`.rake`文件。

```ruby
task :deploy do
  puts "Deploy 1"
end

task :deploy do
  puts "Deploy 2"
end
```

### 给循环加个状态显示
我们Rake任务很多时候会跟循环打交道，这些循环里有些是只读，有些会写入。但无论哪种类型，在循环迭代的时候有个状态输出，总会比死寂一般的hold在那要好很多。

```ruby
task :output do
  %(1..100).each do |x|
    #...
    print '.'
  end
end
```

-完-

### 参考引用
+ [http://t.cn/RIywtrL](http://t.cn/RIywtrL)
