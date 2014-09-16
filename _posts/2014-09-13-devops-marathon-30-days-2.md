---
layout: post
title: Devops马拉松30天——ssh/scp/rsync Day.2
categories:
- Devops
tags:
- linux
- devops
---

###ssh
SSH（Secure Shell Protocol），可以通过数据包加密技术将等待传输的数据包加密后再传输到网络上。默认情况下，ssh协议本身提供两个服务功能

1. 类似telnet可以远程连接到终端shell的服务器，ssh
2. 类似ftp服务器的Sftp-server，提供更安全的文件名传输服务

####安装
{% highlight sh lineos %}

#debian    
sudo apt-get install openssh-server openssh-client   
#centos    
sudo yum install openssh-server openssh-client   

{% endhighlight %}


####使用
启动`/etc/init.d/sshd restart` 同样参数可以为start,stop,该服务同时提供了ssh与sftp两个服务，并且都是在Port22上面

{% highlight sh linenos %}

ssh -f user@ip command   
-f #结合后面的命令起作用，不登陆远程主机直接发送一个命令过去而已   
-o #参数项目(StrictHostKeyChecking=[yes|no|ask] 默认为ask,即要输入`yes`)（ConnectTimeout=秒数：连接等待秒数，减少等待时间）
-t #交互输入密码   
ssh user@ip 'touch abc' #直接执行创建abc      
#如果不写user的话，会以本地计算机的账号来尝试登陆远程

{% endhighlight %}

####几个重要的文件&文件夹
+ `~/.ssh` 存放ssh公私秘钥对、免秘钥认证文件(authorized_keys)、记录已识别主机公钥(known_hosts)   
+ `/etc/ssh/ssh_host*` sshd启动后 服务器提供的公钥与私钥的存放位置  
+ `/etc/ssh/sshd_config` sshd服务器的详细配置文件,几个重要参数含义如下:
> `HostKey /etc/ssh/*`    
> 服务器Private Key放置位置，可以自定义 即上面第二文件所在位置    
> `PubkeyAuthentication yes`    
> 是否允许用户自行使用承兑的秘钥系统登录 仅对Version 2起作用   
> `AuthorizedKeysFile .ssh/authorized_keys`    
> 自定义的公钥数据放置位置(该文件可用于免密码登录)
> `TCPKeepAlive yes`    
> 连接建立以后，服务器会一直发送tcp数据包给客户端，以探听连接是否一直存在,即心跳包 网络不稳定可设置为`no`
+ `/var/log/scure` sshd登录日志存放位置，ssh出了问题时，不要忘记查阅这里，trace error message


对上述配置文件做了修改，一定记得重新启动sshd服务，否则修改不会生效的

#####免密码ssh登陆
即不需要，ssh登录时候每次输入密码，好处是便捷且有利于编写程序脚本，例如crontab定时任务，具体方法是:

1. 客户端建立公私钥对 `ssh key-gen` 一路`Enter`即可 
2. 私钥id_rsa存放到`~/.ssh/`下
3. 公钥id_rsa.pub放置到服务器正确的文件目录的authorized_keys中,所谓的正确目录，可以参看上面配置文件中`AuthorizedKeysFile`所设置的目录，一般默认为`.ssh/authorized_keys`
 
此外ssh这种数据加密的方式还可以应用到原本没有加密的服务中去，即可以在自己的应用中使用该技术，实现加密安全传输

###scp
文件远程直接复制命令，类似于两台机器上的`cp`命令,具体命令格式如下（上传下载区别 颠倒一下主宾,即冒号的位置）:    

`scp [-pr] [-l 速率] local_file user@host:path `

{% highlight sh linenos %}

-p #保留原文件的权限信息     
-r #递归复制   
-l #传输速率限制 单位 Kbits/s   

{% endhighlight %}
#####应用   
`scp -l 800 /root/dd_10mb_file root@127.0.0.1:/tmp`   
限制速度为100Kbytes/s传输 /root/dd_10mb_file 文件到 /tmp   


###rsync 
rsync是一个类似于scp，但不同与scp的强大命令，不同点在于scp是全量复制，而rsync确实增量复制，即只复制两个文件相异的部分，当然这个特性只能体现在头一次复制之后，如果是第一次复制，那么与scp的行为是相同的。其实现的基本算法为：**将待复制文件进行切割成若干块，分别求出hash值，然后对相互之间的块进行比较，只传输相异的部分**，这样可以降低传输消耗(带宽与时间)。

rsync可以使用下面3种传输方式   

1. 本地应用rsync ex: `rsync -av /etc /tmp` 将/etc 备份到/tmp/etc中,注意rsync对`/`是比较敏感的，`/etc` 含/etc及以下文件,`/etc/` 不含/etc仅包含/etc/* 以下的文件
2. 通过ssh信道 远程传输  ex: `rsync -av -e ssh user@host:/etc /tmp` 将host上/etc 备份到本地/tmp/etc中
3. 通过rsync提供的daemon传输,ex: `rsync -av user@host::/etc /tmp`

上面三种方式的异同，在命令上仅仅体现在冒号的个数上，很好记忆~，但第3种方式必须保证rsync服务已经启动

####命令格式
{% highlight sh linenos %}

rsync [-avrlptgoD] [-e ssh] [user@host:/dir] [/path]     

-v #可视模式 显示详细信息     
-q #静默模式 无信息显示    
-r #递归拷贝   
-u #仅更新，不删除   
-l #复制链接文件属性，即不复制源文件   
-p #连同属性一起复制   
-g #保存源文件的文件属性 组 group   
-o #保存源文件的文件属性 主 owner   
-t #保存源文件的时间参数   
-D #保存源文件的设备属性   
-z #数据传输时，加上压缩参数    
-e #使用传输协议 `-e ssh`    
-a #最常用的参数 等价于`-rlptgoD`   ***
--dry-run  #至报告信息 不真实执行 (用语脚本中第一遍checkout)   
--delete #同步删除文件  
--progress #传输时显示传输进度

{% endhighlight %}


####应用场景
增量同步日志文件，服务器之间同步文件，远程镜像备份，特别是同步目录树文件,稍后会再shell部分做一个rsync脚本的解读，看看真实环境中的rsync是怎么应用的



###tricks
觉得每次在ssh中应用，host都要写点分10进的ip地址麻烦，可以尝试 修改 hosts文件 起一个别名，如下:

{% highlight sh linenos %}

vim /etc/hosts
192.168.0.1 linode

{% endhighlight %}


####参考资料
[rsync 的核心算法](http://coolshell.cn/articles/7425.html)   
[《鸟哥的私房菜——服务篇(ssh部分)》](http://linux.vbird.org/linux_server/0310telnetssh.php)

