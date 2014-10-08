---
layout: post
title: git版本库地址变更问题
categories:
- git	
tags:
- git
- linux
---

`fatal: https://..../repo.git/info/refs not found: did you run git update-server-info on the server?`     

使用git push时突然遇到了上面的错误，看了下说是本地记录的git信息与服务器上得不相符了，想了想原来是自己把远程服务器上得repo名字换掉了，因为用的是https协议，所以在推送本地改动到服务端时，找不到原来的repo地址了。

所以只要把不把`origin`地址换成新地址就可以完成正常推送，具体操作执行下面执行:

`git remote set-url origin https://new-address/repo.git`

