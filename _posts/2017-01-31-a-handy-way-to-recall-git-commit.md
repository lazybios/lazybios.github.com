---
layout: post
title: "如何优雅的审视自己过往的Commit"
date: "2017-01-31"
---

我们经常会忙了一周后，回顾时却总是想不起自己到底做了啥？应对这种场景最好的解决方案就是**查看提交日志**，但git自带的log又有那么多参数，输出的东西还不那么友好，那么有没有什么工具能让这一切变得容易一些呢？答案就在今天要说的这个`git-recall`中。先来看看演示图:

![git-recall]({{site.IMG_PATH}}/git-recall.gif)

### 安装
```
cnpm install --global git-recall
```

### 使用
```
$ git recall   [-a <author name>] 
               [-d <days-ago>]
               [-f]
               [-h]
```

+ `-a` - 限制检索用户，使用`-a "all"`，表示不限制用户
+ `-d` - 限制检索时间范围，n天前
+ `-f` - 查看最新修改
+ `-h` - 帮助

### 示例

```
$ git recall
```

默认显示当前用户，昨天到今天发生的commit记录

```
$ git recall -d 5 -a "Doge"
```

显示5天前`Doge`提交过的commit记录

```
$ git recall -d 5 -a "all"
```

显示5天前的commit记录，不限制用户

实际上这个东西除了能做周、月回顾以外，用来追某个特定开源项目的提交记录也是不错的，间隔一段时间的回顾了解这个项目的最新进展。同时它也是个打KPI的好工具。

### 注意
git-recall会依赖一个叫`lesskey`的程序，所以在安装之前，请确保命令行下有这个指令，如果没有可以用`brew install homebrew/dupes/less`重新安装一个less，这个less下会捆绑一个`lesskey`。

-完-

### 参考引用
+ [http://t.cn/Rx85cJh](http://t.cn/Rx85cJh)