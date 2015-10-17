---
layout: post
title: "Rails: 使用Scope进行查询"
date: "2015-10-17 23:27"
---

在模型中通过Scope将常用的查询条件定义成方法，可以在关联对象或模型上调用。能帮我们简化代码的书写。

#### 定义简单作用域

{% highlight ruby linenos %}
class Post < ActiveRecord::Base
  scope :published, -> { where(published: true) }
end
{% endhighlight %}

上面代码等价于在模型中直接定义类方法，其中的`->{}`是lambda函数的简化定义方法

{% highlight ruby linenos %}
class Post < ActiveRecord::Base
  def self.published
    where(published: true)
  end
end
{% endhighlight %}

#### 链式调用
Scope方法支持链式调用，这样我们就能把常用的查询条件组合到一起调用了。

{% highlight ruby linenos %}
class Product < ActiveRecord::Base
  scope :hot, -> { where(hot: true) }
  scope :top, -> { where(top: true) }
end

Product.top.hot.where(name: "T-Shirt")
{% endhighlight %}

#### 参数传递

{% highlight ruby linenos %}
class Post < ActiveRecord::Base
  scope :created_before, ->(time) { where("created_at < ?", time) }
end

Post.created_before(Time.zone.now)
{% endhighlight %}

当有的查询条件需要动态参数的时候，可以采用上面的方法，不过Rails Guides 中推荐如果需要动态参数的时候，不如直接使用类方法，因为有了参数的参与，scope的行为与方法更加接近了，索性不如直接使用方法来定义。


#### 合并Scope
{% highlight ruby linenos %}
class User < ActiveRecord::Base
  scope :active, -> { where state: 'active' }
  scope :inactive, -> { where state: 'inactive' }
end

User.active.inactive
# SELECT "users".* FROM "users" WHERE "users"."state" = 'active' AND "users"."state" = 'inactive'

User.active.where(state: 'finished')
# SELECT "users".* FROM "users" WHERE "users"."state" = 'active' AND "users"."state" = 'finished'
{% endhighlight %}

通过代码可以看出，将两个scope同时链式使用的时候，其对应的查询条件是以AND进行连接的，此外scope也可以直接拿来与where方法使用。

如果一个地方使用了某个 scope，而要在另一个地方把它的条件改变，可以使用 merge 方法：

{% highlight ruby linenos %}
class Product < ActiveRecord::Base
  scope :active, -> { where state: 'active' }
  scope :inactive, -> { where state: 'inactive' }
end

Product.active.merge(User.inactive)
# SELECT "products".* FROM "products" WHERE "products"."state" = 'inactive'
{% endhighlight %}

### 参考引用
+ [http://t.cn/Ryg9GVx](http://t.cn/Ryg9GVx)
+ [http://t.cn/RygKuUo](http://t.cn/RygKuUo)
