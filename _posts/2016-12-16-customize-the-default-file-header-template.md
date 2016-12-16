---
layout: post
title: "自定义Android Studio文件头部模板"
date: "2016-12-16"
---

Andorid Studio会给每个新创建的文件头部自动生成简单的作者信息。但由于是默认配置，所以自动生成的信息内容有限。仅仅包含作者用户名和文件创建时间。不过想让作者信息丰富起来，倒也不是什么难事儿，通过简单的偏好设置就能达成。

##### 操作方法

依次打开「Preferences > Editor > File and Code Templates」找到下图位置，在右侧的模板编辑区内根据自己的需求自定义描述内容并应用即可。

![andorid-studio-header]({{site.IMG_PATH}}/andorid-studio-header.png)

需要说明的是模板里的宏语句都以`#`开头，并且支持自定义变量，你可以使用下面的语法来定义属于你自己的变量，然后再模板中引用。

```
#set ($ORGANIZATION_NAME = "Foobar Ltd")
#set ($EMAIL= "example@domain.com")
```

-完-

### 参考引用
+ [http://t.cn/RIc9ND2](http://t.cn/RIc9ND2)
