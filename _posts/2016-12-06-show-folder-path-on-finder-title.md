---
layout: post
title: "Finder标题栏显示当前文件夹完整路径"
date: "2016-12-06"
---



默认情况下，在Mac中使用Finder浏览文件夹时，标题栏仅显示当前文件夹名字。这对于嵌套不深的文件夹没有问题，但当文件路径嵌套层级较多时，很容易忘记当前所在路径位置。

其实，Finder有一个选项可以在标题栏的位置显示出当前文件夹的具体路径，具体做法只需要在「终端(Terminal)」中根据需要输入下面指令即可。

##### 显示文件夹完整路径
```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
```

##### 隐藏文件夹完整路径
```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool NO
```

![finder-title]({{site.IMG_PATH}}/finder-title.png)

-完-