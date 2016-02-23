---
layout: post
title: "Rails: 通过模型中添加Vitrual Attribute精简控制层代码"
date: "2016-02-23"
---

最近在看Rails Best Practices网站上的文章，虽然文章发布的时间比较久远(最后一篇是4年前发布的)，但里面仍然有大把干货，打算把他们逐个过一遍，顺道会在这里翻译记录一下，有兴趣的同学可以持续关注哈。

### 坏味道的代码

##### 视图层
```erb
<% form_for @user do |f| %>
  <%= text_field_tag :full_name %>
<% end %>
```

##### 控制器层
```ruby
class UsersController < ApplicationController
  def create
    @user = User.new(params[:user])
    @user.first_name = params([:full_name]).split(' ', 2).first
    @user.last_name = params([:full_name]).split(' ', 2).last
    @user.save
  end
end
```
上面代码实现的功能是从视图中的表单提交一个`full_name`字段，在控制层中再手动把它分成`first_name`与`last_name`两部分最后再保存到`user`模型中。

### 重构
但是给Model赋值并不应该是Contoller的职责，正确的做法应该是在Model一层来实现这个功能。

##### 模型层
```ruby
class User < ActiveRecord::Base
  def full_name
    [first_name, last_name]. compact.join(" ").strip
  end

  def full_name=(name)
    split = name.split(' ', 2)
    self.first_name = split.first
    self.last_name = split.last
  end
end
```
##### 视图层
```erb
<% form_for @user do |f| %>
  <%= f.text_field :full_name %>
<% end %>
```

##### 控制层
```ruby
class UsersController < ApplicationController
  def create
    @user = User.create(params[:user])
  end
end
```
视图层依然没有，变化最大的是模型层和控制层，模型中通过添加一个虚拟的属性`full_name=`，来为`first_name`和`last_name`赋值，如此一来，我们可以直接像`create`传入带`full_name`字段的参数，复杂的数据相关逻辑全部丢给了Model来做。

上面的做法其实是要达到一种「fat model, skinny controller」 的效果，除了通过添加虚拟属性的方式之外，还可以借助`attr_accesstor`+callback的方式来实现这一效果，如下代码

##### 控制层
```ruby
class User < ActiveRecord::Base
  attr_accessor  :full_name
  before_save : split_full_name

  private
  def split_full_name
    self.first_name, self.last_name = full_name.splite(" ")
  end
end

```


### 参考引用
+ [http://t.cn/RGKs60A]()

-完-
