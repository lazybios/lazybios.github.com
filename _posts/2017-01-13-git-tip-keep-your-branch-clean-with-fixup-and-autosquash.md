---
layout: post
title: "如何借助fixup与autosquash让Git分支保持整洁"
date: "2017-01-13"
---

我们经常会遇到这样的场景: 一个新特性上线后一段时间没什么问题，但是相隔几个新feature之后渐渐会发现一些小问题，一般面对这样的情形，我们会选择打一个hotfix分支修复后再重新提交。但时间长了Git里的日志中就会存在大量的`fixed type`，`remove xxx`等算不上特性的补丁记录。

显然这样严重降低Git历史的可读性，那有没有办法可以兼顾修复bug的同时，还能保证Git提交记录美观呢？答案显而易见，就是今天要说的`--fixup`与`--autosquash`。配合使用二者，可以给那些无意义的提交找到最佳位置。

### 说明
+ git commit --fixup <commit-sha> 自动在commit消息前添加`fixup!`关键字
+ git rebase -i --autosquash 使用rebase自动合并被标记为`fixup!`的commit，其实是根据sha值来的


### 范例
随便自己创建一个空的repo，不建议在公共分支(master, dev)上操作，因为rebase会修改commit的哈希值，很容易给多人协作带来不必要的麻烦。

```
$ (dev) git add featureA
$ (dev) git commit -m "Feature A is done"
[dev fb2f677] Feature A is done
$ (dev) git add featureB
$ (dev) git commit -m "Feature B is done"
[dev 733e2ff] Feature B is done
```

完成前面两次提交后，你发现了一个针对Feature A的小bug，现在你要去修复它，不过不同于以往的是，你要用`--fixup`去完成最后的commit。

```
$ (dev) git add featureA    # 假设该修复仅仅是删除了一个断点语句
$ (dev) git commit --fixup fb2f677
[dev c5069d5] fixup! Feature A is done
```

此时，通过查看Git的提交日志，你可以看到刚才提交的那个fixup

```
$ (dev) git log --oneline
c5069d5 fixup! Feature A is done
733e2ff Feature B is done
fb2f677 Feature A is done
ac5db87 Previous commit
```

虽然从功能的角度上现在代码已经不报错了。但显然你是个有追求的工程师，你想让分支保持干净整洁，于是你要需要对刚才的fixup提交进行合并，让它遁入到最适合它去的地方。于是你开始用`--autosquash`进行调整了。

```
$ (dev) git rebase -i --autosquash ac5db87
pick fb2f677 Feature A is done
fixup c5069d5 fixup! Feature A is done
fixup c9e138f fixup! Feature A is done
pick 733e2ff Feature B is done
```

就像你看到的那样，这里进行的是交互式的`rebase`操作，所以它会打开一个编辑器让你选择，与单纯的rebase操作不同的是，因为有了`atuosquash`选项，所以Git已经自动帮你甄别出要合并的Commit项了，你需要做的就是确认后保存即可。

![git-fixup]({{site.IMG_PATH}}/git-fixup.png)

到这里，这次修复就算正式完成了，下一步就提交pr等待合并吧。不过，要强调一下的是`--autosquash`后面的Commit哈希参数选择与直接用`rebase`一样，应该选择要合并那个commit的前一个sha值，如果觉得绕不好理解，就好好看本文里的例子吧。

### 参考引用
+ [http://t.cn/RMN5Xb8](http://t.cn/RMN5Xb8)