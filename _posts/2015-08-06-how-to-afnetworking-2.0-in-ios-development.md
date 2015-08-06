---
layout: post
title: "AFNetworking2.0使用小记"
date: "2015-08-06 10:37"
---

在开发iOS APP的过程中，免不了要用到网络功能，这个AFNetworking是绝大多数人的推荐方案，Github上有1.9w的stars,于是乎搞iOS开发学习这个库的使用，也成了不可避免的部分了。

一般的开发中，与服务端的交互常见的会有`GET`和`POST`两种类型的请求方式，当然Restful中，会有更多Action，但使用最频繁就是这两个，今天的Topic就来说说其中之一的`GET`,用AFNetworking库完成`GET`请求的两种方式。(下面的代码直接略过，引入库文件什么的操作了，不过推荐最好用cocoaPods这个包管理工具来引入)

### 方法一

{% highlight objc linenos %}
#import "AFNetworking.h"

static NSString * const BaseURLString = @"http://www.api.com/";

//① 通过字符串构造NSURLRequest对象
NSString *string = [NSString stringWithFormat:@"%@hello?value=world", BaseURLString];
NSURL *url = [NSURL URLWithString:string];
NSURLRequest *request = [NSURLRequest requestWithURL:url];

//② 创建AFHTTPRequestOperation对象并将返回指定返回内容序列号格式,此处为json,可以设置为plist与xml格式
AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
peration.responseSerializer = [AFJSONResponseSerializer serializer];

//③ 设置异步请求处理函数与处理请求失败问题
[operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {

// 异步请求成功后执行，可以在此处存储内容，或者更新UI

} failure:^(AFHTTPRequestOperation *operation, NSError *error) {

// 请求失败会执行的内容

}];

//④ 发起请求
[operation start];

{% endhighlight %}

### 方法二
{% highlight objc linenos %}
#import "AFNetworking.h"

static NSString * const BaseURLString = @"http://www.api.com/";

NSDictionary *parameter = @{
                            @"key1": @"value",
                            @"key2": @"value"
                            }

AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
[manager GET:[NSString stringWithFormat:@"%@hello?value=world", BaseURLString]
  parameters:parameter
     success:^(AFHTTPRequestOperation *operation, id responseObject) {

// 异步请求成功后执行，可以在此处存储内容，或者更新UI

} failure:^(AFHTTPRequestOperation *operation, NSError *error) {

// 请求失败会执行的内容

}];

{% endhighlight %}


上面两个方法都能完成最简单的`GET`请求操作，执行流程也类似，不同的地方是第二种方法，对于参数的管理更加灵活,便于拼凑参数

-待续-


### 参考引用
[http://www.raywenderlich.com/59255/afnetworking-2-0-tutorial](http://www.raywenderlich.com/59255/afnetworking-2-0-tutorial)

