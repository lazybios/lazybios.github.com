---
layout: post
title: 查看浏览器自动保存的密码值
categories:
- linux	
tags:
- firefox
- 浏览器
---


今天又get√一个新技能，单位邮箱密码要求每隔一段时间进行一次更换，更换的久了，新密码很容易忘记，密码寻回流程又比较麻烦，浏览器保存密码免填是个好东西，可以省事儿，但也会让你容易忘记你的密码。

当你忘记密码时，只要通过firebug这样的调试工具，查看下源代码，并把input的`password`类型改为`text`，再回到原来的页面，你就能看到你的密码。是不是很方便，不过话说回来，再公共场合使用自动保存功能一定要谨慎，很容易让自己的账号和密码曝光与众....


![实例图片]({{site.IMG_PATH}}/view-passwd.png) 

