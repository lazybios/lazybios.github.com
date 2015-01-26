---
layout: post
title: Finder中开启显示隐藏文件
categories:
- mac pro
tags:
- os x
---

终端下执行下面命令,开启finder中隐藏文件显示

```
defaults write com.apple.finder AppleShowAllFiles -bool YES
killall Finder #重启finder
```

恢复不显示隐藏文件,设置上面命令布尔值为 `NO`

```
defaults write com.apple.finder AppleShowAllFiles -bool NO
killall Finder #重启finder
```
另外再附一个tricks。在打开或保存文件对话框中,按`Command+Shift+.`(`⌘ + ⇧ + .`),可以切换文件显示状态，不过仅限在呼出的窗口中有效，关闭弹出对话框后，在finder里隐藏文件还是保持不可见的








-完-
