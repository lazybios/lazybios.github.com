---
layout: post
title: "read-the-ruby-china-source-code-day-0"
date: "2015-08-11 17:44"
---

Ruby-China源码追踪: 小贴士功能分析

什么是个小贴士，看这里!

![小贴士]({{site.IMG_PATH}}/RS-tips.png)


小贴士会在PC端的topics列表页与内容页出现,样子如上。既然我们能看到小贴士，那么我们就直接通过`ack`来定位代码位置，`ack 小帖士` 找到小贴士的view文件`app/views/topics/_sidebar_box_tips.html.erb`,发现这里是partial样式，再来通过`ack sidebar_box_tips`都有谁调用了这个partial，结果发现分别是下面两个文件:

+ `app/views/topics/_sidebar_for_topic_index.html.erb`
+ `app/views/topics/show.html.erb`

正好是topic的列表页和内容页，我们再回到`app/views/topics/_sidebar_box_tips.html.erb`文件中看看数据是如何存储和传递的。


``` ruby
<div class="panel panel-default">
  <div class="panel-heading">小帖士</div>
  <div class="panel-body">
    <%= random_tips %>
  </div>
</div>
```

发现此处并没有我们要的结果，而是调用了一个叫`random_tips`方法，这个方法明显不是系统方法，所以可以断定是helper方法
，再来继续追踪`random_tips`,发现确实是在`app/helpers/application_helper.rb`中定义的。


```ruby
def random_tips
  tips = SiteConfig.tips
  return EMPTY_STRING if tips.blank?
  tips.split("\n").sample
```

数据存储在Mongo中的SiteConfig文档从,`SiteConfig.tips`取出数据、判空、返回显示到页面中，这里还有个地方需要指出就是在对于移动端和PC端的显示方法。小贴士的显示逻辑是通过`mobile?`方法判断是否用户客户端类型，过滤显示,view代码如下:


```ruby
<% if !mobile? %>
  <div class="sidebar col-md-3">
    <%= render "sidebar_box_tips" %>
    <%= render "sidebar_box_node_recent_topics", topic: @topic %>
  </div>
<% end %>

```

通过`ack 'mobile\?'`查找方法定义，发现也定义在`app/helpers/application_helper.rb`中。

```ruby
MOBILE_USER_AGENTS =  'palm|blackberry|nokia|phone|midp|mobi|symbian|chtml|ericsson|minimo|' +
                      'audiovox|motorola|samsung|telit|upg1|windows ce|ucweb|astel|plucker|' +
                      'x320|x240|j2me|sgh|portable|sprint|docomo|kddi|softbank|android|mmp|' +
                      'pdxgw|netfront|xiino|vodafone|portalmmm|sagem|mot-|sie-|ipod|up\\.b|' +
                      'webos|amoi|novarra|cdm|alcatel|pocket|iphone|mobileexplorer|mobile'
def mobile?
  agent_str = request.user_agent.to_s.downcase
  return false if agent_str =~ /ipad/
  agent_str =~ Regexp.new(MOBILE_USER_AGENTS)
end
```

原理也简单，使用正则判断reques头部中user_agent类型是否在`MOBILE_USER_AGENTS`里，另外指出的是这里将ipad也按照PC端的方式处理了，想必是因为ipad这类设备的尺寸已经可以当类PC显示了。`MOBILE_USER_AGENTS`列表好全,这个方法可以单独提取出来放到工具箱里，在后面的项目中使用.


小贴士的代码算是追踪完了,对我这个Rails newbie来说学到不少东西。希望后面自己能持续坚持下来！
