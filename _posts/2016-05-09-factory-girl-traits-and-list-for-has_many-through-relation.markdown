---
layout: post
title: "使用Factory Girl Traits表示多对多关系 | Ruby"
date: "2016-05-09"
---

使用Factory Girl Traits表示多对多关系 | Ruby

在开发Web应用时，使用关系型数据库建模，经常出现多对多的关联关系，在Rails中多对多可以用过`has_and_belongs_to_many`与`has_many，through`实现，对于程序编写固然方便，但在测试中又该如何体现它们的关系呢？今天这篇就来说说如何用Factory Girl固件表示`has_many，through`的关系。关系描述如下:

```ruby
class GpsPosition < ActiveRecord::Base
  has_many :gps_trackings
  has_many :modem_registries, through: :gps_trackings
end

class ModemRegistry < ActiveRecord::Base
  has_many :gps_trackings
  has_many :gps_positions, through: :gps_trackings
end

class GpsTracking < ActiveRecord::Base
  belongs_to :modem_registry
  belongs_to :gps_position
end
```

### 使用Traits
Factory Girl自带`trait`关键字，可以使用`trait`来表示一个模型的不同状态，然后利用`after` 钩子方法来创建关联，如下代码

##### FactoryGirl定义
```ruby
FactoryGirl.define do
  factory :gps_position do
    gps_location 'POINT(-83.64753 40.80664)'
  end

  factory :modem_registry do
    wireless_system 'CDMA'

    trait :with_gps_position do
      after(:create) do |modem_registry|
        modem_registry.gps_positions << create(:gps_position)
      end
    end
  end
end
```

##### 使用
```ruby
another_gps_position = create(:gps_position)
create(:modem_registry_with_gps_position, gps_position: another_gps_position)
```

除了`create`外，还可以通过`create_list`来创建整个集合的关联关系，如下:

```ruby
trait :with_gps_positions do
  ignore do
    number_of_gps_positions 1
  end

  after(:create) do |modem_registry, evaluator|
    create_list(:gps_tracking, evaluator.number_of_gps_positions, modem_registry: modem_registry)
  end
end
```

##### 使用
```ruby
create(:modem_registry, :with_gps_positions)
#=> will create 1 modem_registry with 1 gps_position

create(:modem_registry, :with_gps_positions, number_of_gps_positions: 10)
#=> #=> will create 1 modem_registry with 10 gps_positions
```

嗯，如你所见，这篇文章更像是个代码碎片，没错，有时候其实代码比文字更直观，特别是你只想知道该怎么做的时候！最后，还是要提一下，本文是译译自参考引用处的博文，需要看原文自行取阅。

-完-

### 参考引用
+ [http://t.cn/RquknlG]()
