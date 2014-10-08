---
layout: post
title: curl调试http服务
categories:
- linux	
tags:
- curl
- linux
---


开发微信公众平台时，因为公众号与用户的交互都要经过，腾讯的微信服务器post请求，，流程如下图（图是直接从网上down的）

![图片]({{site.IMG_PATH}}/weixin.jpg)

使用curl可以方便的调试公众账号服务端程序，省去了写form表单,直接在命令行下就能轻松搞定，下面记录的是curl的一些基本用法


###下载文件
curl -o [filename] url  

###自动跳转
curl -L url     
会根据跳转指示，跳到最终页面，不会停留到中间页面

###查看http请求详细信息
+ curl -i[I] url    
>-i response header 与 body信息一并显示   
-I 只显示response的header部分

+ curl -v url #显示详细连接建立过程
+ `curl --trace output.txt url` or `curl --trace-ascii output.txt url`    
前者记录详细的连接信息，类似tcpdump的结果，包含16禁止与ascii形式，后者只显示ascii形式的连线详细信息

###提交信息到服务器
####GET方式
{% highlight html linenos %}

<form method="GET" action="junk.cgi">   
 <input type=text name="birthyear">   
 <input type=submit name=press value="OK">   
 </form>   

{% endhighlight%}

+ curl "http://www.hotmail.com/when/junk.cgi?birthyear=1905&press=OK" 


####POST方式

{% highlight html linenos %}

<form method="POST" action="junk.cgi">   
 <input type=text name="birthyear">   
 <input type=submit name=press value="OK">   
 </form>   

{% endhighlight%}

+ curl --data "birthyear=1905&press=%20OK%20"  http://www.example.com/junk.cgi
+ curl --data-urlencode "name=I am Daniel" http://www.example.com     
前者是没有经过编码，需要手动添加编码一些字符，后者curl会自动帮你搞定编码   

###文件上传
{% highlight html linenos %}

<form method="POST" enctype='multipart/form-data' action="upload.cgi">

 <input type=file name=upload>
 <input type=submit name=press value="OK">
</form>

{% endhighlight%}
+ curl --form upload=@localfilename --form press=OK [URL]

###参考
[http://www.ruanyifeng.com/blog/2011/09/curl.html](http://www.ruanyifeng.com/blog/2011/09/curl.html)   
[http://curl.haxx.se/docs/httpscripting.html#The_HTTP_Protocol](http://curl.haxx.se/docs/httpscripting.html#The_HTTP_Protocol)

