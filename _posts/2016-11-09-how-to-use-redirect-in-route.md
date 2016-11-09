---
layout: post
title: "如何在路由层处理跳转 | Rails"
date: "2016-11-08"
---

![rails_logo]({{site.IMG_PATH}}/rails_logo.png)

有时候我们需要处理一些站外的跳转请求，比如集成某个第三方功能啥的。比较直接的做法是专门为这些站点创建个`action`来处理这些逻辑，虽然这样做可以解决问题，但为一个跳转专门维护一个方法貌似有点杀鸡用牛刀的感觉。其实Rails有在路由层提供跳转的方法。


```ruby
get '/stories' = > redirect('/posts')
```
上面是最简单的写法，如果待跳转的链接是固定的，那么这样写就可以了，但如果跳转链接中还包含动态值，则可以用下面的写法:

```ruby
get 'docs/:id', to: redirect('/wiki/%{id}')
```

虽然上面的写法已经能应对很多场景了，但还是不够完善，如果待跳转链接中包含参数或需要加入些逻辑处理，上面两种写法就力不从心了。这时可以考虑用代码块的方式，手动的嫁接参数。

```ruby
get 'jokes/:number', to: redirect { |params, request|
  path = (params[:number].to_i.even? ? "foo" : "bar")
  "http://#{request.host_with_port}/#{path}"
}
```

```ruby
get '/foo',to: redirect { |params, request| 
  "/pages/#{request.params[:page]}.html?#{request.params.to_query}"
}
```

需要指出的是虽然语法块中的内容超过一行，但这里只能使用语法块的行内表示`{...}`，否则会产生歧义，误把语法块的内容传给`get`方法，而不是`redirect`。

-完-

### 参考引用
+ [http://t.cn/RfZgIUR]()
+ [http://t.cn/RfZd3hT]()