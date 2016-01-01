---
layout: post
title: "Rails: 四个Layout与Render的应用技巧"
date: "2016-01-01"
---


看到一篇不错的关于Layout和Render技巧的文章，感觉挺实用的，所以简单的意译了一下拿来跟大家分享。想看原文的老规矩文末直接穿越就好，直接贴正文了希望能帮到你。:)

### 根据条件显示布局
```erb
# app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
 <head>
   <!-- head content -->
 </head>
 <body>
   <%= render 'layouts/header' %>
   <%= yield %>
   </body>
</html>
```
如果上面的Header部分只是想针对一些特定页面展示，可以把上面的render部分改成如下代码:

```erb
# app/views/layouts/application.html.erb
.....
  <%= render('layouts/header') unless headless %>
.....
```
之后在Controller中响应的Action中可以通过设置`headless`变量来决定是否显示header部分。

### 一套代码多个布局
Layout在Rails中比较灵活，可以在Controller级别进行指定，也可以在Action层级做限定。默认如果在Controller层做指定，那么所有方法都会应用该layout，但action则不同，所以我们可以利用这一特性来解决诸如由于权限的限制，授权用户与非授权用户所显示的界面不一样的问题，代码如下:

```erb
# app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
 <head>
   <!-- head content -->
 </head>
 <body>
   <%= render('layouts/header') unless headless %>
   <div class="page">
       <%= render 'layouts/sidebar' %>

       <div class="page-content">
         <%= yield %>
       </div>
     </div>
   </body>
 </html>
```

```erb
# app/views/layouts/guest.html.erb
<!DOCTYPE html>
<html>
  <head>
    <!-- head content -->
  </head>
  <body>
    <%= render 'layouts/login_bar' %>
    <%= yield %>
   </body>
 </html>
```

```ruby
# app/controllers/guest_controller.rb
class GuestController < ApplicationController
  # change layout for all controller actions
  layout 'guest'
  def signin
  end
  def signup
     # change layout on per action basis
     render layout: 'guest'
   end
 end
```

### 利用代码块提取公共内容
有时候我们也可以利用`render`方法的代码块来提取一些公共内容，比如HTML页面中的`head`部分，利用代码块和`yield`关键字就可以把`head`部分提取成一个partial，示例代码如下:

```erb
# app/views/layouts/_head.html.erb
<!DOCTYPE html>
<html>
 <head>
   <!-- head content -->
 </head>
 <body>
   <%= yield %>
     <%= render 'layouts/footer' %>
   </body>
 </html>
```

```erb
# app/views/layouts/application.html.erb
<%= render :layout => 'layouts/head' do %>
 <%= render('layouts/header') unless headless %>
 <div class="page">
   <%= render 'layouts/sidebar' %>
   <div class="page-content">
     <%= yield %>
     </div>
   </div>

 <% end # render partial layout 'head' %>
```

```erb
# app/views/layouts/guest.html.erb
<%= render :layout => 'layouts/head' do %>
 <%= render 'layouts/login_bar' %>
 <%= yield %>        
<% end # render partial layout 'head' %>
```


### 根据页面指定Assets文件
有时候我们不想让样式文件一股脑的集成显示到一起，而是想针对不同的页面内容引用
不同的样式文件，这时其实可以用`content_for`来由选择的显示assets文件，如下示例代码。

```erb
# app/views/layouts/guest.html.erb
<%= render :layout => 'layouts/head' do %>
 <% content_for :layout_assets do %>
   <!-- layout specific assets -->
 <% end %>
 <%= render 'layouts/login_bar' %>
 <%= yield %>        
 <% end # render partial layout 'head' %>
```

```erb
# app/views/guest/login.html.erb
<% content_for :template_assets do %>
 <!-- template specific assets -->
<% end %>
<!-- template content -->
```

### 参考引用
+ [http://t.cn/R4JSZhW]()
