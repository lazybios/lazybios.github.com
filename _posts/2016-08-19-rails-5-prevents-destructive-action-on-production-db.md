---
layout: post
title: "阻止Rake指令意外清空线上数据库 | Rails 5"
date: "2015-08-18 18:16"
---

有时候我们会在线上环境中调试问题，不过因为是调试所以难免要修修这改改那，然后就会一不留神用`RAILS_ENV=production rake db:schema:load`这样的命令把数据丢了。

为了应对上面可能出现的意外情况，Rails 5 中会用一张`ar_internal_metadata`表在执行Migration的过程中把要保护的数据库环境信息存储到其中，然后在执行高危动作时自动对环境做侦测预判。

当我们第一次执行`rake db:migrate`时，`ar_internal_metadata`会把`production`存入其中。之后再执行`rake db:schema:load`或`rake db:structure:load`这类命令时，Rails会检查当前环境变量是否显示的包含`production`。如果没有包含`production`的环境信息，Rails就会抛出异常并阻止数据被清洗。

当然如果你执意要那么干，可以通过将`DISABLE_DATABASE_ENVIRONMENT_CHECK=1`做为待执行命令的参数之一传递出去从而改变Rails自检的行为。

```ruby
$ rake db:schema:load

rake aborted!
ActiveRecord::ProtectedEnvironmentError: You are attempting to run a destructive action against your 'production' database.
If you are sure you want to continue, run the same command with the environment variable:
DISABLE_DATABASE_ENVIRONMENT_CHECK=1
```

类似上面的例子，自检不仅对`db:schema:load`与`db:structure:load`有效，对于`db:drop`、`db:purge`也同样适用。另外其实你也可以在自定义的oneoff task中使用该技术。

-完-

### 参考引用
+ [http://t.cn/RtmPTFk]()
