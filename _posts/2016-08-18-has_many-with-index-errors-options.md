---
layout: post
title: "has_many中的index_errors选项 | Rails 5"
date: "2016-08-18"
---

当我们需要在一个Form表单中处理多个Model的时候，一般会用`accepts_nested_attributes_for`方法，它可以帮我们轻松搞定关联关系的属性。

Rails 4.x中，如果提交表单的过程中的某个关联模型没有通过验证，往往是没办法在表单中显示出错误信息的。

```ruby
class Product < ApplicationRecord
  has_many :variants
  accepts_nested_attributes_for :variants
end

class Variant < ApplicationRecord
  validates :display_name, :price, presence: true
end

>> product = Product.new(name: 'Table')
>> variant1 = Variant.new(price: 10)
>> variant2 = Variant.new(display_name: 'Brown')
>> product.variants = [variant1, variant2]
>> product.save
=> false

>> product.error.messages
=> {:"variants.display_name"=>["can't be blank"], :"variants.price"=>["can't be blank"]}
```

基于上面的示例代码，如果我们直接通过JSON API访问出错，因为存在多个`variants`，但错误消息仅仅提到了`variants`，并没有具体的序号或ID，所以是无法定位到具体出错对象的。

不过，对于常规通过HTML渲染的表单则不存在这种问题，因为错误消息都会依附于每个独立的`variants`实例对象之上，所以可以很容易的找出到底是哪个variants导致的问题。

### Rails 5中支持通过索引定位出错对象
Rails 5中，通过`index_errors: true`参数为`has_many`关联的模型对象增加了索引值，从而可以允许我们在错误信息中定位到出错对象的具体序号。

```ruby
class Product < ApplicationRecord
  has_many :variants, index_errors: true
  accepts_nested_attributes_for :variants
end

class Variant < ApplicationRecord
  validates :display_name, :price, presence: true
end

>> product = Product.new(name: 'Table')
>> variant1 = Variant.new(price: 10)
>> variant2 = Variant.new(display_name: 'Brown')
>> product.variants = [variant1, variant2]
>> product.save
=> false

>> product.error.messages
=> {:"variants[0].display_name"=>["can't be blank"], :"variants[1].price"=>["can't be blank"]}
```

### 开启全局设置
如果想要在全局配置中开启关联索引选项，可以在`application.rb`中进行如下设置:

```ruby
config.active_record.index_nested_attribute_errors = true
```
之后，模型中就没必要再通过`index_errors: true`声明了。

-完-

### 参考引用
+ [http://t.cn/RtExCDS]()
