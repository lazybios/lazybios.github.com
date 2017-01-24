---
layout: post
title: "git-diff美化工具diff-so-fancy"
date: "2017-01-19"
---

之前文章里推荐过一款命令行diff美化工具，今天再给安利一枚专门针对git-diff的命令行美化工具。

### 安装
diff-so-fancy是一个用node实现的命令行工具，所以推荐使用`npm`来安装。

```
npm install -g diff-so-fancy
```

### 使用
直接在命令行调用

```
git diff --color | diff-so-fancy
```

这种用法虽然简单，但会把所有结果一股脑全部输出，查看起来就显得没重点了。比较好的做法是通过设置pager对结果进行分页

```
git config --global core.pager "diff-so-fancy | less --tabs=4 -RFX"
```

不过即使这样，每次都敲那么多命令也是不合理的，所以最好通过下面办法设置一个命令别名:

```
git config --global alias.dsf '!f() { [ -z "$GIT_PREFIX" ] || cd "$GIT_PREFIX" '\
'&& git diff --color "$@" | diff-so-fancy  | less --tabs=4 -RFX; }; f'
```

这样我们再查看diff时，就可以直接用`git dsf commit-sha`了。

![diff-so-fancy]({{site.IMG_PATH}}/diff-so-fancy.png)

是不比起默认的样式，这种花花绿绿的感觉好多了呢？(笑) 除了上面最基本用法外，diff-so-fancy还支持颜色和显示项定制。有兴趣的可以自己到官方文档里翻翻看。



-完-

### 参考引用
+ [http://t.cn/RMgygeG](http://t.cn/RMgygeG)