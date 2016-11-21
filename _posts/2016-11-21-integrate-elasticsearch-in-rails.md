---
layout: post
title: "在Rails中使用Elasticsearch| Rails"
date: "2016-11-21"
---

对于大多日常简单的检索需求，其实Mysql的SQL语句也能搞的定，不过如果检索中的筛选条件日益复杂，写出来的SQL也会越来越冗长难于维护，这时不如考虑引入Elasticsearch专门来做搜索，它所提供的Restful API，以及清晰的JSON返回结果，不仅没啥学习门槛，而且使得维护和调试也更简单了。

### 安装

系统中装好并启动ElasticSearch后，分别添加下面两个Gem到Gemfile，执行`bundle install`安装即可。

```ruby
gem 'elasticsearch-model'
gem 'elasticsearch-rails'
```

### 使用
`elasticsearch-rails`这个项目中包含三个子Gem，它们的功能如下:

+ elasticsearch-rails 封装了一些ES与Rails框架密切相关的操作
+ elasticsearch-model 封装了与`ActiveRecord`以及`Mongoid`集成的接口
+ elasticsearch-persistence 就如persistence表示的那样，使用它可以将ES直接当做持久存储层

一般我们只会用到前两个Gem，所以在安装时候只安装这两个就可以了。


elasticsearch-model就位之后，我们可以通过Mixin的方式将`Elasticsearch::Model`引入需要添加搜索功能的Model中以定义相应的Mapping(类似关系数据库中的Schema)，这样这个模型就初步的具有简单的检索功能了。

```ruby
require 'elasticsearch/model'

class Article < ActiveRecord::Base
  include Elasticsearch::Model
end
```

不过，往往我们实际中的需求要更复杂一些，比如我们需要Model中的数据在索引之后发生数据变动自动更新索引，又或者不希望所有的字段都被索引，仅检索固定有限的几个字段即可。

##### app/models/concerns/searchable.rb

```ruby
module Searchable
  extend ActiveSupport::Concern

  included do
    include Elasticsearch::Model

    # 根据情况调整下
    mapping dynamic: false do
      indexes :content
      indexes :author
      indexes :remark
      # ...
    end

    after_commit do
      __elasticsearch__.index_document
    end

    def self.search(query)
      # ...
    end
  end
end
```

##### app/models/article.rb
```ruby
class Article
  include Searchable
end
```

类似上面的需求，比较好的做法是把ES搜索相关的逻辑提取成一个单独的Concern，这样可以被复用到任意的模型中。默认情况下Elasticsearch::Model会给模型的全部字段创建索引，所以你可以根据实际需求在Searchable中做一些具体的调整。

完成上面模型的设置后，还需要一步就能实际的检索了，那就是实际的为模型在ES中创建索引。不过创建索引页很简单，这回我们要用elasticsearch-rails提供的rake任务来快捷的创建索引。首先创建一个elasticsearch.rake文件，并写入下面内容：

##### lib/tasks/elasticsearch.rake

```ruby
require 'elasticsearch/rails/tasks/import'
```
##### Rake导入指令
```sh
rake environment elasticsearch:import:model CLASS='Article' FORCE=y
```

索引导入成功后，就可以在Controller中使用`Article.search`方法进行检索了。

```ruby
response = Article.search 'fox dogs'

response.results.to_a
response.records.to_a
```

嗯，到这里ES与Rails之间就算是全部打通了，可以基本满足大多数常见场景。不过，这并不算完，ES与Rails之间还有很多进阶的用法，比如对检索结果进行分页，调整索引配置，对多个模型之间同时进行检索，以及对检索结果进行更加精细的调优等，后面有机会的话会继续这个主题来写的。

-完-

### 参考引用
+ [http://t.cn/RfaKHHM](http://t.cn/RfaKHHM)