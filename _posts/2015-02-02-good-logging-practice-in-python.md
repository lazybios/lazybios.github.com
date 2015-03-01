---
layout: post
title: [译]Python日志最佳实践[未完]
published: false
categories:
- Python
tags:
- logging
- python
---

记录日志在真实使用是很重要。当你汇款，会有专门的汇款记录。飞机航行，会有一个叫黑匣子(航行数据记录仪)
记录所有得数据。如果哪里出了问题，可以通过阅读日志找出问题所在。同样地，日志对于系统开发，调试以及运行
都至关重要。当程序运行时崩溃掉，如果没有日志，机会没有可能搞清发生了什么。例如，如果你在写一个服务程序，那么日志是
很有必要的。下面得截图是[EZComet.com](http://ezcomet.com/)服务的一份日志文件

![datatype4rails-01]({{site.IMG_PATH}}/2015-02-02.png)

没有日志，如果服务挂掉，机会很难找到是哪里出错了。不仅仅是服务程序，记录日志对于桌面GUI应用也同样重要。例如，当你的程序在客户端机器上奔溃掉了，你可以要求用户把日志文件发送过来，通过日志文件你可以知道哪里出了问题，并且能找到解决方案。相信我，你永远无法知道程序在一个不同得pc环境会发生什么奇葩问题。我曾经受到过一份错误日志，如下：


{% highlight  sh nos %}

2011-08-22 17:52:54,828 - root - ERROR - [Errno 10104] getaddrinfo failed
Traceback (most recent call last):
File "<string>", line 124, in main
File "<string>", line 20, in __init__
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/wx._core", line 7978, in __init__
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/wx._core", line 7552, in _BootstrapApp
File "<string>", line 84, in OnInit
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.wxreactor", line 175, in install
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet._threadedselect", line 106, in __init__
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.base", line 488, in __init__
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.posixbase", line 266, in installWaker
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.posixbase", line 74, in __init__
File "h:\workspace\project\build\pyi.win32\mrdj\outPYZ1.pyz/socket", line 224, in meth
gaierror: [Errno 10104] getaddrinfo failed

{% endhighlihgt %}

最后才高清楚客户的电脑被感染病毒，导致`gethostnmae`失败，看到了吧，如果没有看日志，我们怎么会知道这种事情的发生?

### 使用`print`函数不是个好主意
尽管打印日志是件很重要的事儿，但并不是所有得开发人员都知道如何正确使用它们。我看到一些开发人员通过在程序中插入`print`语句，开发结束后再删除它们。它们可能看起来像这样：

{% highlight sh nos%}

print 'Start reading database'
records = model.read_recrods()
print '# records', records
print 'Updating record ...'
model.update_records(records)
print 'done'

{% endhighlight %}

这样的方式在程序还只是个简单地脚本的时候起作用，但对于复杂的系统，你最好不要使用这种方法。首先，你不可能只把重要的信息存放到日志中。你可能会看到大量的垃圾消息，但是找不到任何对自己有用的信息。同样你不可能通过不修改代码来控制这些打印语句。你也许会忘记删掉那些无用的`print`语句。并且所有的打印信息都被输出到了标准输出中，这样当你有其它有用的信息想要输出到屏幕，你就会发现这样多糟糕。当然你可以打印信息到标准错误输出。但是同样得道理。使用`print`语句真的不是什么好的日志实践。

### 使用logging Python标准模块
那么，你该如何正确地记录日志呢？使用Python标准logging模块。感谢Python社区，logging是一个标准模块，它被设计的使用非常简单和灵活。你可以使用[logging system]像这样:

{% highlight python nos %}

import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

logger.info('Start reading database')
# read database here

records = {'john': 55, 'tom': 66}
logger.debug('Records: %s', records)
logger.info('Updating records ...')
# update records here

logger.info('Finish updating records')

{% endhighlight %}

执行改脚本，你会看到

{% highlight python nos%}

INFO:__main__:Start reading database
INFO:__main__:Updating records ...
INFO:__main__:Finish updating records

{% endhighlight %}

你可能会问，这和`print`方法有什么不同。好吧，当然有不同，会有如下好处：

+ 你可以控制消息等级和过滤那些不重要得信息
+ 你可以决定在什么地方或者如何输出

你可以使用`debug`,`info`,`warning`,`error`及`critical`几个不同的等级。通过给`logger`与`handler`不同的权重级别,你可以像特定的日志文件只写入错误信息，或者在debug时候记录debug详细信息。让我们改变logger级别为`DEBUG`再看一次输出。

`logging.basicConfig(level=logging.DEBUG)`

输出为：

{% highlight python nos%}

INFO:__main__:Start reading database
DEBUG:__main__:Records: {'john': 55, 'tom': 66}
INFO:__main__:Updating records ...
INFO:__main__:Finish updating records

{% endhighlight %}

正如你看到的，我们调整日志等级为`DEBUG`，之后records信息出现在了输出中。你也可以决定如何处理这些信息，比如，你可以使用一个`FileHandler`类把记录写到一个文件中

{% highlight python nos%}
import logging

logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)

# create a file handler

handler = logging.FileHandler('hello.log')
handler.setLevel(logging.INFO)

# create a logging format

formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)

# add the handlers to the logger

logger.addHandler(handler)

logger.info('Hello baby')

{% endhighlight %}

logging提供有不同的`handlers`,可以把记录发送到你的邮箱中，或者甚至发送到一个远程服务器上。你也可以实现你急的logging handler。我不打算进行这方面细致的描述，可以参考这些官方文档:[基础教程](http://docs.python.org/2/howto/logging.html#logging-basic-tutorial),[高级教程](http://docs.python.org/2/howto/logging.html#logging-advanced-tutorial)以及[Logging Cookbook](http://docs.python.org/2/howto/logging-cookbook.html#logging-cookbook)

### 使用合适的level记录日志
得益于logging模块的灵活性，你可以设置适当的级别，以及配置它们后把日志打到任何地方。那么你会问如何使用level才属适当。下面我来分享一下我的一些经验。

在大多数的案例中，你不会想要在日志文件中阅读太多细节。因此，debug模式仅仅在你要进行调试的时候开启。只有在需要详细调试信息，特别是当数据较大或者使用比较频繁，例如某个算法for循环过程中的内部状态转换。

{% highlight python nos%}

def complex_algorithm(items):
for i, item in enumerate(items):
# do some complex algorithm computation

logger.debug('%s iteration, item=%s', i, item)

{% enghighlight %}

对于一些例行内容，我一般使用info这个级别。比如，处理访问请求或者服务状态转换

{% highlight python nos%}

def handle_request(request):
logger.info('Handling request %s', request)
# handle request here

result = 'result'
logger.info('Return result: %s', result)

def start_service():
logger.info('Starting service at port %s ...', port)
service.start()
logger.info('Service is started')

{% enghighlight %}

warning 一般用在比较重要，但是不是错误，比如，当一个用户试图以错误得密码进行登录，或者连接缓慢的时候

{% highlight python nos%}

def authenticate(user_name, password, ip_address):
if user_name != USER_NAME and password != PASSWORD:
logger.warn('Login attempt to %s from IP %s', user_name, ip_address)
return False
# do authentication here

{% enghighlight %}

error level用在发生某些错误的时候，比如，异常抛出，IO操作失败或者连接问题
{% highlight python nos%}

def get_user_by_id(user_id):
user = db.read_user(user_id)
if user is None:
logger.error('Cannot find user with user_id=%s', user_id)
return user
return user

{% enghighlight %}

我很少使用critical，你可以在某些特别坏得事情发生时使用，比如，内存溢出，硬盘满了或者核泄漏什么的(希望它不会发生 :S)

### 使用`__name__`作为logger的名称
你没有必要非得把logger的名字设置为`__name__`，但是这样做，确实会带来一些好处。`__name__`变量存放的是当前模块的名字。例如，在`foo.bar.my_module`模块中调用`logger.getLogger(__name__)`，等价与`logger.getLogger("foo.bar.my_module")`。当你需要配置这个logger的时候，你可以配置"foo",这样所有得得在"foo"这个包里地模块的会共享到这个配置。同样当你阅读日志的时候你会明白什么是模块消息。

### 捕获异常并且记录追踪它们
在某些地方出错的时候记录下它们算是一种最佳时间，但是如果没有记录错误追踪信息其实用处也不会很大。你应该在捕获异常的同时，记录下错误的追踪信息，下面为例：

{% highlight python nos %}

try:
    open('/path/to/does/not/exist', 'rb')
except (SystemExit, KeyboardInterrupt):
    raise
except Exception, e:
    logger.error('Failed to open file', exc_info=True)

{% endhighlight %}

通过在调用logger方法的时候带一个`exc_info=True`参数，traceback信息会被转存到logger中，你将会看到下面的结果:

{% highlight sh nos %}

ERROR:__main__:Failed to open file
Traceback (most recent call last):
File "example.py", line 6, in <module>
open('/path/to/does/not/exist', 'rb')
IOError: [Errno 2] No such file or directory: '/path/to/does/not/exist'

{% enghighlight %}

也可用过调用`logger.exception(msg,*args)`,等价于`logger.error(msg,exc_info=True,*args)`

### 不要在模块级定义logger除非`disable_existinglogger = False`

### 参考引用
[http://victorlin.me/posts/2012/08/26/good-logging-practice-in-python](http://victorlin.me/posts/2012/08/26/good-logging-practice-in-python)


-完-
