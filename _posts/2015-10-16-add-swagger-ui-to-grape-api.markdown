---
layout: post
title: "Rails: 使用Swagger来维护Grape API文档"
date: "2015-10-16 21:04"
---

Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。其将维护接口文档的工作，与维护代码相结合，使得二者可以齐头并进，随着代码文件的更新，文档也得到了更新。

![Ruby元编程]({{site.IMG_PATH}}/swagger.png)

+ 修改Gemfile,添加下面Gem

```ruby
gem 'grape', '0.10.0'  
gem 'grape-swagger', '0.9.0'
gem 'grape-swagger-rails'
```

+ API存放路径结构

```sh

app
├── api
│   ├── dispatch.rb
│   └── v1
│       └── test_api.rb

```

+ 创建`config/initializers/grape_swagger_rails.rb`，添加如下内容

```ruby
GrapeSwaggerRails.options.url      = "api/swagger_doc"
GrapeSwaggerRails.options.app_name = 'appname'
GrapeSwaggerRails.options.app_url  = '/'
```

+ 修改routes.rb文件，挂载路由

```ruby
mount Dispatch, at: 'api'
mount GrapeSwaggerRails::Engine => '/docs'
```

+ 在dispatch.rb文件中添加`add_swagger_documentation`方法

```ruby
add_swagger_documentation(
  base_path: "/api",
  hide_documentation_path: true,
  hide_format: true,
)
```


上面的步骤做好以后，就可以通过`http://locahost/docs`访问文档了。这里需要注意的是有的较早一点的文章中将`GrapeSwaggerRails.options.url`会设置为`api/swagger_doc.json`，这样的会导致js解析接口的时候出现404错误，去掉后缀即可，另外如果还有其它奇奇怪怪的错误，可以通过devtools来排错。

-完-
