---
layout:
title: iOS开发从Stroyboard到xib
categories: []
tags: []
published: True

---

####1. 新建项目
1. Create a new Xcode project
2. iOS -> application -> Single View Application
3. 填写信息，next -> create

####2. 去掉Stroyboard引导
1. 点击项目 -> TARGETS -> Main Interface (delete tab键)

####3. 补充xib文件
1. cmd + n 创建一个新xib文件
2. iOS -> User interface -> view
3. 命名后确定存储位置，Create

####4. 配置xib文件
1. 点击上一步中创建的xib文件中的 黄色立方体 File's Owner
2. 打开 检查器3 cmd + option +3 (Identity Inspector)
3. Custom Class  > 填写相应Controller -> 按Tab
4. 打开检查器6 cmd + option +6 Connections Inspector
5. Outles -> view -> 点击空白圈 拖动到xib中view处放手

####5. AppDelegate 补充代码
1. 在AppDelegate.h 中导入头文件 `#import "ViewController.h"`
2. 在AppDelegate.h 中添加 属性 `@property (strong, nonatomic) ViewController *viewController;`
3. 在AppDelegate.m 中添加启动代码

```
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.viewController = [[ViewController alloc] initWithNibName:@"ViewController" bundle:nil];
    self.window.rootViewController = self.viewController;
    [self.window makeKeyAndVisible];

```
