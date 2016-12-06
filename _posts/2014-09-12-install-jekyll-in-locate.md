---
layout: post
title: 安装Jekyll本地调试环境
categories:
- Jekyll
tags:
- linux
- blog
---

学了ruby一大截了，自己也终于忍受不了git log上毫无意义的"fixed last bug"这样的注释了，索性在本地搞一个预览版，每次把格式，内容在本地预览编辑好再提交到github上，同时有错误发生也能看到本地build的日志，很快的定位错误，不用根据github build邮件里的提示，乱猜了-.-

其实过程还是比较简单的，并且github本身也有help文档作为指导，做把搬运工，看不明白的请直接到这里[查阅](https://help.github.com/articles/using-jekyll-with-pages#installing-jekyll)

####具体步骤
+ 确保你的`ruby --version`为 1.9.3 or 2.0.0 (if not,pls google by yourself to install ruby with right version)    
+ 安装bundler包管理器 `gem install bundler`   
+ 在你的site(github-page) 主目录下创建`Gemfile`,该文件内的具体内容如下:

```sh
source 'https://rubygems.org'      
gem 'github-pages'
```

+ 执行`bundle install`

以上你就已经顺利的在本地安装好了与github-page对应的版本的jekyll，之后你需要做的就是**在你的site根目录下**执行`bundle exec jekyll serve`启动jekyll，默认访问地址为：`http://localhost:4000`，这样你就顺利的得到了一个本地版的github-page镜像:)

最后你可以随时更新你本地的jekyll，通过`bundle update`。

