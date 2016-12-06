---
layout: post
title: Git回顾笔记(二)
categories:
- Git
tags:
- git
- notes
---
继续上面那篇git笔记

#### git mv 命令
`$ git mv hello.rb lib`        
使用git mv 相当与重命名，该指令执行后直接进入stage不许要再手动操作   


####.git 文件夹
`.git/objects`文件夹里面包含的是commit生成的sha1值的对应文件夹，文件名就是hash值的前两个字母    
`.git/config` 文件中存放的是当前版本库中的配置文件，该文件与`～/.gitconfig`文件功效相同，只是作用域不同，在固定的版本中前者的优先级高于后者。会覆盖.gitconfig中的部分内容。    
`.git/refs/heads`与`.git/refs/tags`两个文件中分别存放的是分支和标签。   

####--amend 参数
这个我体会最深，我在维护blog的过程中发布后经常会有一些格式问题，以前我的做法是修改后再做一次commit，这样我的分支中就多了很多没有意义的`fixed`log，导致整个主干分支变得特别丑陋。有了这个--amend参数，可以修改上一次commit的内容和commen t     
`$ git add hello.rb`    
`$ git commit --amend -m "Add an author/email comment"`    
之后再通过`git st`查看的时候，就会发现之前的comment和内容都发生了改变，这对于偶尔的小bug修改特别有意义，当然这个效果也可一通过使用reset命令来实现，不过就是麻烦一些    

####histroy的--all参数
`git hist --all` 可以显示所有分支的log信息，而不局限于某个分支。    

####merge合并
某些合并后会发生冲突，这时需要手动去调整，下面冲突文件为例子    

```ruby
<<<<<<< HEAD
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
  please contact us at support@github.com
</div>
>>>>>>> iss53
```

这个冲突会新生成的文件，把冲突的两部分做了划分，`=======`作为分界线，HEAD表示当前分支，iss53表示另一个分支，手动处理选择正确的部分保留，同时删除`<<<...`和`>>>...`以及`===...`部分，之后可以再次进行add,commit，这样就算完成了本次合并      

