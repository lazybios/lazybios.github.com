---
layout: post
title: "Bundler与Gemfile最佳实践"
date: "2017-02-01"
---

##### Gemfile.lock is for apps, not libraries
`Gemfile.lock`是未了保证运行环境一致性而产生的，这样可以最大限度的减少因为环境差异所造成的运行异常。但如果你写的是个library，那么你则应该抛弃`gemfile.lock`以确保library的通用性。

##### Only specify top-level dependencies
一般gem都会在spec里写明其所依赖的gems有哪些，并且`bundler`在安装的时候自动的分析他们之间的关系，所以大可不必去关心其中的底层依赖，放心的交给bundler好了。

##### Use Gemfile groups
开发过程中尽管你会依赖很多gem，但并不代表这些gem都应该在线上环境被加载。相当一部分其实只是为了提高你的开发效率存在，所以最好把它们区别出来加以分组，比较好的是分成三组：开发、测试、线上。

##### Consistent formatting
Gemfile应该跟你的程序代码一样被严格规范起来:


+ 使用清晰的缩进
+ 使用单引号字符串
+ 检查多余的空格字符
+ 合理的功能分区，以及注释说明,`3rd party apis, performance, devops...`
+ 避免用单行group定义，尽量用blocks语法

>
```
# Good
group :development do
  gem 'web-console'
  gem 'spring'
end
```
```
# Bad
gem 'web-console', group: :development
gem 'spring', group: :development
```


##### Resist the urge to Ruby
Gemfile有自己的DSL，所以不要在Gemfile里炫技使用Ruby语法，这里的炫技并不能帮你提升什么性能，写的通俗易懂才是大众喜闻乐见的。不过天朝环境下这个用法很好用:

```
if ENV['USE_OFFICE_GEM_SOURCE']
  source 'https://rubygems.org'
else
  source 'https://gems.ruby-china.org'
end
```

##### Minimize git dependencies
尽管Gemfile允许你直接通过Git来安装gem，但还是不推荐你再Gemfile大量使用这种方式，原因有三:

1. 长时间的依赖私有库，会让你偏离该gem的主分支，今儿会错过很多必要的更新和一些新特性
2. 相比语义化的版本而言，git提供的hash值让人很难搞懂版本之间的关系
3. 更好的解决方法是给原gem提issue、patch，这样可以营造一个更好的开源环境

##### Do you really need that gem?
是否添加一个新依赖到应用中应该是件需要慎重决定的事儿。gem带来便利的同时也会增加维护成本，还会拖慢你的应用。

> Every dependency in your application has the potential to bloat your app, to destabilize your app, to inject odd behavior via monkeypatching or buggy native code.


-完-

### 参考引用
+ [http://t.cn/Rxufxws](http://t.cn/Rxufxws)