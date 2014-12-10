---
layout: post
title: scrapy爬取分页的小技巧
categories:
- Python
tags:
- scrapy
- python
---

#### 爬取分页

其实超级简单,在spider中间中根据规则设置start_urls,具体的callback函数还是parse()

{% highlight python %}

start_urls = ["http://xxx.com/page/%s" nu for nu in xrange(1,20)]


{% endhighlight %}

此外两个方向是使用`yield`关键字代替`return`，另外一个是使用CrawlSpider代替BaseSpider，定义rule规则，这两个我也没有实践,就不多说了，有痛点得自行探索一下，另外说一个`download_delay`参数

#### DOWNLOAD_DELAY
DOWNLOAD_DELAY这个参数用来，伪装调节访问速度，为防止触发服务器策略被禁掉ip，单位为秒，等待的时长同时受到`RANDOMIZE_DOWNLOAD_DELAY`的影响，即当你访问统一个站点的时候，其间隔时间是一个随机变量，
介于0.5和1.5*DOWNLOAD_DELAY直接，默认该设置是开启的，如果DOWNLOAD_DELAY设置为0，则该项设置无效！
