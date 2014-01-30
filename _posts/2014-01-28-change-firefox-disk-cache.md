---
layout: post
title: 修改FireFox缓存路径及/run相关
categories:
- Linux日常
tags:
- linux
- firefox
- ubuntu
---

当时装ubuntu时候，/home文件容量过小，用firefox看在线视频时缓存就用侵占/home容量导致同时其它文件就不能同时进行写操作，想到改下缓存路径，搜了下看到了一篇介绍利用/run/shm共享内存做firefox硬盘缓存的文章，试一试

`df -h`查看/run/shm路径大小   
firefox导航栏输入`about:cache`查看disk cache device路径与大小，删除对应路径下无用Cache，导航栏在输入`about:config`添加一个String类型的新条目，输入`browser.cache.disk.parent_directory` 对应文件路径则为`/run/shm`,重启浏览器即可    
firefox依次进入`edit-preferences-andvances` Network 标签，勾选`Override automatic cache management` 同时设置Limit cache大小为250MB，不要沾满/run/shm盘，够看视频缓存就行，因为/run/shm给的数据是最大值，实际在使用时会有浮动     

另外对用/run文件相关查阅了下资料，大致功能是提供临时文件存储，包括不同进程之间设备锁和共享内存      

/run 对应临时文件系统(tmpfs)存放临时文件，属于RAM，不可永久保存,挂掉硬盘上也就是虚拟内存了     

/run/lock 包含锁文件，容许不同进程间访问共享设备   
 
/run/shm 共享内存,不同进程间IPC通信

/run也就是swap，设置大小最好是内存的50% ,同时可以使用`ipcs -m`结合`df -h`查看进程的具体使用情况
     



