---
layout: post
title: "使用Factory Girl表示多态关联 | Rails"
date: "2016-05-09"
---

使用Factory Girl表示多态关联 | Rails

除了多对多的关联关系外，Web开发中也有大量的多态关联，比如评论，图片这种公共资源，一篇文章可以有图片，有评论，反过来图片的留言其实也可以有嵌套的评论，以及插图。Rails中多态可以通过`polymorphic: true`轻松实现(如下代码)，但我们又要问了在测试中该如何体现这种关系呢？今天这篇延续昨天的文章，继续来看下如何在测试中用Factory Girl表示多态关联。

#### 模型的多态实现
```
class Profile
  belongs_to :profileable, polymorphic: true
end

class Account
  belongs_to :accountable, polymorphic: true
end

class Student
  has_one :profile, as: :profileable
  has_one :account, as: :accountable
end

class Teacher
  has_one :profile, as: :profileable
  has_one :account, as: :accountable
end
```

#### Account与Profile的Factory Gril定义
```
FactoryGirl.define do
  factory :account, class: Account do
    email                  { Faker::Internet.email }
    password               { "supersecret" }
    password_confirmation  { "supersecret" }
    association :accountable
  end
end

FactoryGirl.define do
  factory :profile, class: Profile do
    first_name             { Faker::Name.first_name }
    last_name              { Faker::Name.last_name }
    association :profileable
  end
end
```

#### Student的Factory Girl定义
```
FactoryGirl.define do
  factory :student, class: Student do
    trait :trial          { subscription_status: 0 }
    trait :subscribed     { subscription_status: 1 }

    after(:create) do |student|
      create(:account, accountable: student)
      create(:profile, profileable: student)
    end

  end
end
```

上面代码，其中`Faker`打头的是`faker`这个Gem提供的测试数据，能让你的测试数据更真实一些，另外比较值得关注的是`association`和callback方法`after`，前者用来描述关联，后者通过hook来创建关联，是不是与前一篇文章中描述多对多关联的语法很相似呢，不过与`association`结合后它便可以轻松表示多态关联了。

-完-

### 参考引用
+ [http://t.cn/Rq1WzaN]()
