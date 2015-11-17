---
layout: post
title: "CoffeeScript 101"
date: "2015-11-17 21:42"
---

最近正在切换到新的工作环境，需要对之前的好多东西做整理。所以预计将会有一大波的101系列袭来，不过不要紧这些新坑都是管挖也管埋。今天主题是Coffeescript 101，先来简单介绍一下CoffeeScript。

### Coffeescript简介
首先要把握住一点，CoffeeScript本质上还是JavaScript，只是书写的方式不同而已，最后的`.coffee`都会被编译为`.js`文件，只有这样的格式才能被浏览器所接受。之所以要使用这么一种中转语言，是因为它能帮助我们从js单调的语法中解放出来，CoffeeScript的语法上集成了Python和Ruby的许多优点，写起来会越写越轻松，并且经过CoffeeScript生成的JavaScript代码都是被优化过的。

#### CoffeeScript语法特性列表
+ 使用代码缩进取代了原语法中的大括号，写起来跟Python差不多
+ 有参数的时候可以省略函数的括号，无参数的情况不能省略括号，因为要与普通变量进行区分
+ 不用分号做结尾，根据换行来区分语句
+ 不用显示标明return语句
+ 添加了许多贴近语言的语法关键字，例如`unless`,`is`,`isnt`

![比较运算符列表图]({{site.IMG_PATH}}/coffeescript-101.png)

### 安装和使用
```
brew install node
npm install -g coffee-script
```

有两种方式编辑CoffeeScript代码，一种是通过编辑`.coffee`后缀文件，另一种是直接在类似`ipyhton、irb`这样的交互式解释器(通过`coffee`命令进入)书写。

使用`.coffee`文件，需要通过命令行将CoffeeScript编译为js文件，因为本质上你写的还是js代码，只是换了一种写法而已，与SCSS的性质是一样的。

```
coffee -c -w .
```

使用上面命令可以将命令执行时所在目录下的`.coffee`后缀的文件都转为`.js`，其中`-w`表示持续监听源文件的变化，如果发现文件有变化就自动再编译一遍`.coffee`。


#### Node与浏览器中定义全局变量
```
# nodejs中
global.GlobalVariable = "foo"

# 浏览器中
window.GlobalVariable = "bar"
```

#### 遍历数组与对象的区别
```
# 遍历数组
for item in array

# 遍历对象
for k, v of author
```
从代码中就能看出二者的区别来，当遍历数组时使用`in`关键字，当遍历对象用的却是`of`关键字，后者会在一次遍历中同时取到两个值，属性的键和属性的具体值。


-待续-

#### 参考引用
+ [《CoffeeScript 应用开发》]()
