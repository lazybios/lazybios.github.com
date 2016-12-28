---
layout: post
title: "Android API Level与SDK的关系"
date: "2016-12-28"
---

安卓开发中除了要面临适配兼容各厂商ROM外，搞明白Google自家的Android发行版本号也是非常重要的。这样在正确选择compileSdkVersion、minSdkVersion以及targetSdkVersion时就不会抓瞎了。

+ compileSdkVersion 表示编译程序时会用哪个版本的SDK
+ minSdkVersion 应用可以运行的最低要求，用户安装应用的时候会以此做为能否安装的基准
+ targetSdkVersion 为Android提供向下兼容，targetSdkVersion没有更新之前，不会应用新SDK中的新特性

#### 选取公式
```
minSdkVersion <= targetSdkVersion <= compileSdkVersion
```

有了这个公式之后，剩下的就是结合Android的发行版本号，以及你APP的目标群体特点，选择恰当合适的API Level去开发，编译你的应用。到目前为止所有的版本号都在下面这张表里。


| Code name  |  Version | API level  |
|:-:|:-:|:-:|
| Nougat | 7.1 | API level 25 |
| Nougat | 7.0 | API level 24 |
|Marshmallow | 6.0	| API level 23|
|Lollipop |	5.1|	API level 22 |
|Lollipop	|5.0 |	API level 21 |
|KitKat |	4.4 - 4.4.4	|API level 19|
|Jelly Bean|4.3.x|	API level 18|
|Jelly Bean|4.2.x|	API level 17|
|Jelly Bean|4.1.x|API level 16|
|Ice Cream Sandwich	|4.0.3 - 4.0.4|	API level 15, NDK 8|
|Ice Cream Sandwich	|4.0.1 - 4.0.2|	API level 14, NDK 7|
|Honeycomb|	3.2.x|	API level 13|
|Honeycomb|	3.1	|API level 12, NDK 6|
|Honeycomb|	3.0	|API level 11|
|Gingerbread|	2.3.3 - 2.3.7|	API level 10|
|Gingerbread|	2.3 - 2.3.2	|API level 9, NDK 5|
|Froyo|	2.2.x|	API level 8, NDK 4|
|Eclair|	2.1	|API level 7, NDK 3|
|Eclair|	2.0.1|	API level 6|
|Eclair|	2.0	|API level 5|
|Donut|	1.6|	API level 4, NDK 2|
|Cupcake|	1.5|	API level 3, NDK 1|
|(no code name)|	1.1	|API level 2|
|(no code name)|	1.0	|API level 1|

-完-

### 参考引用
+ [http://t.cn/RInLeW1](http://t.cn/RInLeW1)
+ [http://t.cn/R490c5T](http://t.cn/R490c5T)