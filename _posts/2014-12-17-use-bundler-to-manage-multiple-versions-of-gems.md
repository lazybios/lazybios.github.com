---
layout: post
title: 使用bundler管理多版本的Gem
categories:
- rails
tags:
- rails
- ruby
- bundler
---

有了rbenv来管理多版本的ruby环境，我们还需要一个能管理多版本gem(比如rails)的工具，那就是bundler了，项目背景不细说了，需要了解的直接到[官网](http://bundler.io/),这里只讲一些实际使用经验

###安装
`gem install bundler`

###使用
{% highlight sh nos %}

mkdir app1; cd app1;
echo "source 'https://ruby.taobao.org/'" > Gemfile
echo "gem 'rails,'4.1.0'" >> Gemfile
bundle install

{% endhighlight %}

上面代码在app1下安装了`rails 4.1.0`,使用`bundle exec rails -v`查看当前目录下使用的rails版本，显示内容应该为`Rails 4.1.0`,同样此时通过`bundle exec rails new . --force`覆盖原来Gemfile,此时的app使用的rails版本为4.1.0

{% highlight sh nos %}

mkdir app2; cd app2;
echo "source 'https://ruby.taobao.org/'" > Gemfile
echo "gem 'rails,'3.2.13'" >> Gemfile
bundle install

{% endhighlight%}

上面代码创建了第二个app2文件夹，并通过bundler安装了`rails 3.2.13` 同样通过`bundle exec rails new . --force`可以生成基于rails 3.2.13版本的应用

安装了以上两个版本后，通过`gem list --local`可以看到rails有两个版本，显示为`rails (4.1.0, 3.2.13)`,bundler会智能的判断每个项目的rails版本，以确保应用的正确运行，但前提是通过使用`bundle exec`命令来执行原来得命令，例如:

{% highlight sh nos %}

bundle exec rails s
bundle exec rake db:create
...

{% endhighlight %}


-完-
