---
layout: post
title: "使用大括号扩展(Brace Expansion)减少重复输入"
date: "2017-01-24"
---

众所周知`tab`补全与`ctrl+r`检索是命令行下两个不需要额外配置就可以提高生产效率的好法宝，不过本文今天要分享的是另一个与上述二者有相同功效的命令行小技巧——大括号扩展(brace expansion)。

### String lists
##### 使用前
```
mv playground/rigonyizu/today.md playground/rigongyizu/tomorrow.md
```

##### 使用后
```
mv playground/rigongyizu/{today,tomorrow.md}
```


是不是一目了然，公共部分被提取了出来，最大限度的减少了重复输入。其实**大括号扩展**除了可以结合`mv`外，还可以配合诸如`cp、rm、touch`使用，甚至是`vim, wget`这些指令。所以当你遇到上面这样有冗余输入时，不妨试一试。

```
wget http://docs.example.com/documentation/slides_part{1,2,3,4,5,6}.html
```

### Ranges
除了可以通过提取公共部分减少额外输入外，还可以借助`Range`功能，自动计算序列化的文件名，比如下面这样:

##### 使用前
```
touch file1.md
touch file2.md
touch file3.md
```

##### 使用后
```
touch file{1..3}.md
```

这个是比较简单的用法，不过貌似也是比较常见的，除了支持数字序列，还可以使用字母序列`{A..Z}`，以及简单的排列组合`{A..Z}{0..9}`这样，具体用到自己探索吧。

### 注意
+ `{}`里的`,`后面没有空格



-完-

### 参考引用
+ [http://t.cn/Rx4hLAm](http://t.cn/Rx4hLAm)