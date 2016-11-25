---
layout: post
title: "使用dotenv管理环境变量"
date: "2016-11-25"
---

项目开发过程中，经常会涉及到类似数据库密码、第三方服务密钥等敏感信息。对于这些信息我们往往不会把它们直接写到codebase里，通常的做法是将它们以环境变量的形式传递。尽管这样能很好的解决敏感信息泄露的问题，但当这类数据有了规模，团队人员日益变多，修改Profile文件管理环境变量的方法就变得不那么奏效了，那么有没有一种集中高效易于分享的管理方式呢？答案就在dotenv这个Gem里。

### 安装
```ruby
gem 'dotenv-rails', :groups => [:development, :test]
```

在Gemfile中添加上面内容，执行`bundle install`即可。


### 使用

最直接的用法是在Rails项目根目录创建一个`.env`文件，将涉及敏感信息的内容以下面形式写到里面。

```ruby
S3_BUCKET=YOURS3BUCKET
SECRET_KEY=YOURSECRETKEYGOESHERE
```

当遇变量值为多行时，可使用`\n`转义字符表示，如下:

```ruby
PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----\nHkVN9…\n-----END DSA PRIVATE KEY-----\n"
```

变量的内部也支持计算，使用Bash语法`$(command)`可以动态的构造字符串变量值

```ruby
DATABASE_URL="postgres://$(whoami)@localhost/my_database"
```

此外，也可以在变量名前像在bashrc中定义变量一样，前面加一个`export`，这样这个`.env`文件就可以通过`source`的方式批量定义环境变量，以供其它bash脚本使用。

```ruby
export S3_BUCKET=YOURS3BUCKET
export SECRET_KEY=YOURSECRETKEYGOESHERE
```

上面介绍的主要是定义方法，访问其实没啥需要介绍的，跟使用通过`bashrc`定义的环境变量一样，通过`ENV`变量即可加载。

```
config.fog_directory  = ENV['S3_BUCKET']
```


### 注意事项
在Rails中，`.env`变量通过回调`before_configuration`进行加载，这意味着你只有在`Application`定义以后才能访问到它们。如果你有场景需要在这之前加载环境变量，那么只能自己手动操作。

```ruby
# config/application.rb
Bundler.require(*Rails.groups)

Dotenv::Railtie.load

HOSTNAME = ENV['HOSTNAME']
```

另一个需要注意的地方是，当你用了需要依赖环境变量才能加载的Gem时，你需要把`dotenv-rails`置于该Gem之前，并且设置`require`参数如下:

```ruby
gem 'dotenv-rails', :require => 'dotenv/rails-now'
gem 'gem-that-requires-env-variables'
```

-完-

### 参考引用
+ [http://t.cn/RfpoeKO](http://t.cn/RfpoeKO)
