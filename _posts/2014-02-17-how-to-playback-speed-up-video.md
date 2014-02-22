---
layout: post
title: Ubuntu下视频加速播放
categories:
- Liunx日常
tags:
- ubuntu
- mplayer
---

经常看一些MOOC上的视频，考虑到一些视频冗长无趣的问题，所以很有必要加速播放，一直没有找到一款在linux下合适的播放器可以做到播放加速音质不变的，所以之前的视频好多不得不转回windows看，来回切换真的是很麻烦的说。今天终于被我找到了——mplayer！(笑)

其实也很简单，首先就是安装mplayer
> `sudo apt-get install mplayer`

之后设置音频过滤器，未设置过滤器默认情况加速会有走音现象，另外mplayer加速快捷键`[`，减速快捷键`]` 
> `mplayer -af help` #查看已安装的mplayer是否包含scaletempo音频过滤器,若在列表则可用，一般不是太早的mplayer版本都自带的      
> `mplayer -af scaletempo xxxx.mp4` #使用过滤器播放相应视频

使用scaletempo音频过滤器会使播放器根据播放速度自动调节音频声调，使音质尽量接近自然语调     

`-af`参数即aduio filter,具体其它mplayer快捷参数，详细请参考mplayer mannual   

   

  







