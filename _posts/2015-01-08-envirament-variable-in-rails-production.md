---
layout: post
title: Rails部署环境的环境变量
categories:
- rails
tags:
- rails
- ruby
---

如果你是头一次部署rails到生产环境，并且也是一个ruby newbie那一定会被rails配置文件中的ENV["variable_name"]的写法搞得莫名其妙，
最大得问题可能就是哪里来得这些变量？这篇文章就来说说这些环境变量的作用以及简单地使用方法

###环境变量的作用
rails中环境变量与系统中环境变量一样，特定环境下才能访问到得变量，可以保护开发者敏感信息(密码，hash key之类)，其次可以
提高部署灵活性(依赖部分的路径定义)。


###环境变量的配置和访问
#### 访问
可以直接通过`ENV['variale_name']`这种写法访问到，设置过的环境变量

#### 配置
+ 通过`.bashrc`配置（`.zshrc`类似）
{% highlight sh nos %}

export ELASTICSEARCH_URL="xxxxxx"
export DATABASE_PASSWORD="xxxxxx"

{% endhighlight %}

+ 通过yaml文件配置
新建yaml文件，在rails app启动的时候首先加载环境变量配置文件，从而达到覆盖追加原系统变量的目的
**新建ymal文件**

{% highlight ruby nos %}
#config/local_env.yml

ELASTICSEARCH_URL: "xxxxxx"
DATABASE_PASSWORD: "xxxxxx"

{% endhighlight %}

**rails启动前加载静态yml文件**

{% highlight ruby nos %}
#config/application.rb

config.before_configuration do
  env_file = File.join(Rails.root, 'config', 'local_env.yml')
  YAML.load(File.open(env_file)).each do |key, value|
  ENV[key.to_s] = value
  end if File.exists?(env_file)
end

{% endhighlight %}

**修改.gitignore**

添加`/config/local_env.yml` 到.gitignore中，放置配置文件随代码一起泄露出去

除了上面的两个方法，还可以通过通过一个叫`Figaro`gem来实现环境变量设置，而且可以针对不同开发环境进行设置，暂时还没有这么精细的需求用不到，详细可以跳到参考链接里看

### 参考
[Rails 环境变量设置](http://angryz.github.io/2014/03/13-rails-environment-variables/)

很好的一篇文章，大部分参考了该文，推荐！

-完-
