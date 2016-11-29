---
layout: post
title: "设置Finder中显示隐藏文件的快捷方式"
date: "2016-11-29"
---

对于终端用户而言查看隐藏文件应该不是啥问题`ls -a`一下就都出来了，但如果是要在Finder中显示可能就要稍微麻烦一些了。

其实最快捷的方法就是打开Terminal应用，然后输入下面指令:

##### Finder中显示「隐藏文件」
```
defaults write com.apple.finder AppleShowAllFiles -bool true
killall Finder
```


##### Finder中隐藏「隐藏文件」
```
defaults write com.apple.finder AppleShowAllFiles -bool false
killall Finder
```

-完-
