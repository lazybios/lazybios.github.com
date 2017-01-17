---
layout: post
title: "无人值守自动安装Ubuntu安全升级"
date: "2017-01-17"
---

Ubuntu的更新迭代速度感人，为了保持服务器时刻能处在最新安全的状态，最好是将服务器配置为无人值守自动升级模式(安全前提下的)。

### 安装

```
$ sudo apt-get install unattended-upgrades
$ sudo dpkg-reconfigure -plow unattended-upgrades
```
执行后进入交互界面，选择Yes->OK就可以了。

![automatic-update-1]({{site.IMG_PATH}}/automatic-update-1.png)

![automatic-update-2]({{site.IMG_PATH}}/automatic-update-2.png)

### 配置更新内容

默认情况下只安装安全相关的更新，不过你也可以根据你的需要修改下面路径文件，以决定要自动安装哪些更新：

```
/etc/apt/apt.conf.d/50unattended-upgrades
```

```
Unattended-Upgrade::Allowed-Origins {
        "${distro_id}:${distro_codename}";
        "${distro_id}:${distro_codename}-security";
//      "${distro_id}:${distro_codename}-updates";
//      "${distro_id}:${distro_codename}-proposed";
//      "${distro_id}:${distro_codename}-backports";
};
```


### 设置更新频率
默认情况下更新频率是一天一次，可以在`/etc/apt/apt.conf.d/20auto-upgrades`文件里修改这个设置，文件里的1表示一天，换成你想设置的天数即可。

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

-完-


