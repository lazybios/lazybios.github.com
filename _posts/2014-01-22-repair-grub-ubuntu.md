---
title: Linux Grub修复
layout: post
categories: linux
tags: 
- ubuntu 12.04
- grub
---

昨天晚上清理旧的系统内核文件时，不慎将所有内核都删掉了，我还以为当前版本内核应该不后被删掉的，删除后因为没有即时补救直接reboot了，结果就悲剧了:(
再启动，发现grub菜单没有了，只有一个光标在那里一直一闪一闪的...

这里说明下当前版本内核被删掉其实是内核的基础文件以及一些配置文件，之所以没有立即产生影响是因为在linux启动时内核已经被加载到了内存里面，言下之义就是如果你没有像我一样急急忙忙的reboot，完全可以`sudo apt-install linux-image-xxx.xxx`在从新安装回来，不会有任何影响

先说下我的配置环境
> 双系统，win7+ubuntu 12.04 主引导用的是windows的bootleader

#####修复步骤
1. 用系统LiveCD，光盘也好，usb的也可以，只有能启动到安装界面，进入试用ubuntu就行
2. 终端，切到root下操作，摸清硬盘分区情况
> `su` #无需root密码    
`fdisk -l` 查看分区情况，搞清/boot是独立挂载还是跟/目录在一起，二者有少许区别

3. 挂载/，/boot目录到当前livecd 系统中
> `mount /dev/sda8 /mnt` # 挂载/目录到/mnt 另外这里搞清个概念/dev/sda指硬盘(ex：/dev/sdb，不同的两块硬盘)，/dev/sda1~n表示分区，根据自己情况填写    
`mount /dev/sda7 /mnt/boot` #挂载/boot到/mnt/boot ,/boot单独分区，可以略过此步

4. `grub-install --boot-directory=/mnt/ /dev/sda`  #更新重装grub到指定目录，这里注意是--boot，不是--root,网上很多经验贴都是--root,不知到怎么得来的，我是行不通，自己`--help`看下参数说明


到这里，reboot下，可以看到grub菜单的同学恭喜你，不过我的到这里还不行，只是不再仅一个光标bling-bling了，grub可用了，仅命令行交互式的，下面继续...

5. `help`能看到所有内建指令，`ls`会给出当前分区情况，hd0~n表示的，执行下面指令,仍然独立/boot分区和不独立还是有稍许不同
> /boot独立执行    
`set root=(hd0,7)` #(hd0,7)即/boot所在分区，具体视你的情况而定    
`set prefix=(hd0,7)/grub`  
`insmod /boot/grub/normal.mod` #装载normal.mod模块   

> /boot在/目录下执行   
`set root=(hd0,7)` #(hd0,7)即/boot所在分区，具体视你的情况而定   
`set prefix=(hd0,5)/boot/grub` #仅路径上的小区别而已   
`insmod /boot/grub/normal.mod` #装载normal.mod模块    

6. `normal` #执行noraml，重启，这时终于见到了原来的grub菜单,但工作并没完成，除非你想每次这样手动进入linux (笑)

7. 进入linux，终端操作更新grub列表，并从新安装一边grub再，这样完全恢复到原理状态，靠grub菜单进入linux
> `sudo update-grub`   
> `sudo grub-install /dev/sda` #注意这里是标识装到硬盘a,如果你要装到硬盘b，那就换参数为/dev/sdb     
> `reboot` #well,done 大功告成    

过程真的很费劲啊，拥有权限的情况下删除一类操作一定要三思,否则就等着收拾残局吧!好在我当时reboot前补救了一把，升级了内核，不过与当前系统不匹配，所以可能因此搞坏了grub，如果内核一个都没有，我岂不又得折腾的更新内核镜像:(

参考阅读:
[inux系统启动流程及 MBR损坏,grub内容,文件误删,boot目录,分区误删修复 ](http://tanxin.blog.51cto.com/6114226/1167151)
[win7 ubuntu10.10 重装win7 丢失grub菜单问题解决方法 ](http://blog.csdn.net/hitprince/article/details/7400759)     
[双系统或三系统:Grub Rescue修复方法 ](http://blog.csdn.net/undoner/article/details/17095611)

