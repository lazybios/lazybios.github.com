---
layout: post
title: "javascript:void(0) 的含义"
date: "2016-11-16"
---

在Stackoverflow上见到的这个问题，看了下问题的赞数，相信有不少同学应该也是这样总见但不知道为啥吧。

```html
<a href="javascript:void(0)" id="loginlink">login</a>
```

正常情况下，这个`href`的值应该是一个URL地址，但有时候我们会把一个`a`标签当做一个按钮来处理，给它绑定相应事件，当我们触发这个按钮事件的时候，我们当然不希望触发到`a`标签的默认跳转事件，这时我们就可以把`javascript:void(0)`来设为`href`的属性。

`javascript:void(0)`分成两个部分，前面的`javascript:`表示它是一个URI(资源定位符)，你可以把它想象成像http那样的协议名。后面的`void`是一个运算符，这个运算符能够执行其中给定的表达式。这里的`void(0)`执行后能得到一个`undefined`的值。

当我们点击一个`javascript: ` 链接时，浏览器会对冒号后面的代码进行求值，然后把求值的结果显示在页面上，这时页面基本上是一大片空白，这通常不是我们想要的。只有当这段代码的求值结果是 undefined 的时候，浏览器才不会去做这件傻事，所以我们经常会用 void 运算符来实现这个需求。

-完-

### 参考引用
+ [http://t.cn/RUzCLOa](http://t.cn/RUzCLOa)
+ [http://t.cn/Rfc3JIA](http://t.cn/Rfc3JIA)