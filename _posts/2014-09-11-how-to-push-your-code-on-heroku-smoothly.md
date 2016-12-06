---
layout: post
title: 如何顺利的完成heroku的首次push
categories:
- Ruby
tags:
- Ruby on Rails
---


还是[CS169.1x](http://www.xuetangx.com/courses/UC_BerkeleyX/CS169_1x_1/)的话题，HW2要求将一份代码部署到真实的生产环境中，这里的生产环境即[Heroku](https://heroku.com)。在群里讨论中，发现大家因为对linux环境的不熟悉，加之对ruby也比较陌生，特别是引入了新的开发框架--Ruby on Rails，其代码的框架就很难让人搞明白(刚开始0.o)，所以难免会遇到一些问题在部署的过程中，而且在这其中还引入了“第三者”--Git(学习git，可以看[这里](http://lazybios.github.com/2014/05/relearn-git-of-notes/)),这就更加让大家摸不清所犯错误的出处在哪里，上一篇中，就有介绍即将面对的坑，索性我就把这个过程记录一下，希望能帮大家避开这个坑，专心到ROR的学习(撒花...)

####问题描述
在你继续下面的问题研究前，请确保你已经正确的在本地安装好了heroku-toolbelt(未部署成功，请参考[这里](http://freshstu.com/2014/09/repair-heroku-toolbelt-install-failure-in-ubuntu11-10/)),并且已经按照作业的tips中的描述完成了ssh-key的部署，真正的走到了`git push heroku master`这一步

在执行push命令的过程中，你会遇到下面的问题：

```ruby
 !
 !     Precompiling assets failed.
 !     Attempted to access a nonexistent database:
 !     https://devcenter.heroku.com/articles/pre-provision-database
 !

 !     Push rejected, failed to compile Ruby app
```

根据提示显示是预编译失败，试图访问一个不存在的数据库，其实已经说的很明白了，不过需要注意的是heroku官方表示不支持sqlite3(也不能说完全不支持，只是你要用sqlite3会造成你的数据不一致而且不能做到数据持久化，起不到数据库的效果)，所以我们就需要换一个heroku支持的，官方推荐的是postgresql，于是我们需要对代码中数据库配置的部分做一个修改，另对于穿越的同学，框架代码可以在[这里](http://www.xuetangx.com/c4x/UC_BerkeleyX/CS169_1x_1/asset/rails-intro.zip)下载到

####具体措施
+ 官方推荐在本地也要安装一套postgresql，那我们就照做一下(不装如何，你可以试一试 我也没尝试过...) 执行 `sudo apt-get install postgresql-client postgresql`
+ 修改应用主目录下的config/database.yaml对应部分为：(修改时，注意yaml的格式，不要把那空格给丢了)

```
production:
  adapter: postgresql
  database: my_database_production
  pool: 5
  timeout: 5000
```

+ 最后为App添加postgresql模块 `heroku addons:add heroku-postgresql`

上面三步完成就顺利`git push heroku master`，部署你的应用代码了。

最后说说虚拟机的事儿，本身课程设立虚拟机就是为了给你规避那些你目前学习saas的过程中不必要的问题，以防让你分心，否则课程为什么不从装linux开始教你，但有的同学偏偏要本末倒置，在课程的分支上越走越远，最后可能你的linux是大有进步，但那并不能帮你完成现阶段的目的，最后可能折腾半天课程还是不能做好，不是你没下对工夫，只能说明你不得要旨...

####参考资料
1. [heroku上sqlite说明](https://devcenter.heroku.com/articles/sqlite3)
2. [heroku预备数据库说明](https://devcenter.heroku.com/articles/pre-provision-database)
3. [CS169.1x HW 2代码框架下载](http://www.xuetangx.com/c4x/UC_BerkeleyX/CS169_1x_1/asset/rails-intro.zip)(非答案仅代码框架)
