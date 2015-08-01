---
layout: post
title: "MPMoviePlayerController全屏模式下横屏与竖屏切换"
date: "2015-08-01 10:37"
---

使用系统的MPMoviePlayerController处理多媒体视频文件的时候，会遇到在进行全屏切换时，进入全屏后播放画面会发现还是竖屏模式，这样的话就完全失去了全屏模式的有优势了，浪费了很多屏幕空间，查阅了一些资料，发现可以通过注册监听通知中心关于MPMoviePlayerController控件的进入全屏(MPMoviePlayerWillEnterFullscreenNotification)和退出全屏(MPMoviePlayerWillExitFullscreenNotification)消息，判断当前应用所处状态，之后根据状态来设置屏幕的方向,从而对屏幕旋转方向进行控制。

#### AppDelegate.h文件中引入MediaPlayer头文件

{% highlight objc linenos %}
#import <MediaPlayer/MediaPlayer.h>
{% endhighlight %}

#### 声明记录当前应用状态变量

{% highlight objc linenos %}
@implementation AppDelegate
{
        BOOL _isFullScreen;
}
...
@end
{% endhighlight %}

#### 注册进入全屏和退出全屏消息事件

{% highlight objc linenos %}
[[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(willEnterFullScreen:)
                                                 name:MPMoviePlayerWillEnterFullscreenNotification
                                               object:nil];
[[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(willExitFullScreen:)
                                                 name:MPMoviePlayerWillExitFullscreenNotification
                                               object:nil];
{% endhighlight %}


####  实现上面消息对应的事件响应函数

{% highlight objc linenos %}
- (void)willEnterFullScreen:(NSNotification *)notification
{
    _isFullScreen = YES;
}

- (void)willExitFullScreen:(NSNotification *)notification
{
    _isFullScreen = NO;
}
{% endhighlight %}

#### 实现`application:supportedInterfaceOrientationsForWindow:`方法

{% highlight objc linenos %}
- (NSUInteger)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
{
    if (_isFullScreen) {
        return UIInterfaceOrientationMaskPortrait | UIInterfaceOrientationMaskLandscapeLeft | UIInterfaceOrientationMaskLandscapeRight;
    } else {
        return UIInterfaceOrientationMaskPortrait;
    }
}
{% endhighlight %}

按照顺序完成上面几个步骤就可以实现播放器进出全屏后的屏幕方向旋转了。

-完-

#### 参考
[http://stackoverflow.com/questions/20285356/mpmovieplayercontroller-can-rotate-in-full-screen-while-the-application-only-sup](http://stackoverflow.com/questions/20285356/mpmovieplayercontroller-can-rotate-in-full-screen-while-the-application-only-sup)
