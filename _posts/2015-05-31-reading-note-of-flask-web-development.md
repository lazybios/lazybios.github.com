---
layout: post
title: 《Flask Web开发：基于Python的Web应用开发实战》 读书小记
categories: []
tags: []
published: True

---

读[《Flask Web开发：基于Python的Web应用开发实战》](http://book.douban.com/subject/26274202/)的源起是因为需要为一个Web app开发一套符合REST API,因为有遗留数据库的问题，而且只是单独的需要一套接口而已，所以果断没有采用Rails，Django这样的大家伙，看一下大家的方案，推荐使用flask的居多，浏览了下官方文档，就果断决定入Flask的坑了。后来在豆瓣上找到了这本中文译作，通篇看一遍，比较偏应用实战，符合我的口味！书的结构也比较合理，分成3部分，分别是Flask的基本理论篇、实战应用篇、后期部署维护篇。这篇读书笔记主要用来记录那些散落在书里的一些对我有用的知识点，以备后查，希望恰巧也能帮助到你。


安装什么的就不赘述了，忘记了直接到官网查询就好了，不过需要注意的是最好使用`virtualenv`来安装，这样不仅可以很好地保护开发机上的库环境防止版本冲突问题，同时也能很好地规避安装包时的文件权限问题。

```
sudo apt-get install python-virtualenv #ubuntu中安装virtualenv
virtualenv env_name #创建env_name环境
cd env_name && source bin/activate  #激活env_name环境
deactive #关闭env_name环境，回到系统环境

```

安装好了之后，自然是希望能顺利的跑起来，看看效果，那么你估计需要一个这样最小的范例来给你些信心！

```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()

```

上面就是一个可以直接运行的范例，确保安装完flask之后，你可以直接复制这个脚本，命名为`hello.py`,通过`python hello.py`,然后根据提示就能在浏览器访问了。

一旦你能正常跑起这个应用，你会很快不满足于当前这个比较low的应用。有过其它语言或框架的同学，一定会开始追问，我该如何post表单、如何渲染模板，如何与数据库交互，以及如何请求调度。这些自然难不倒Flask这个微框架。

对于表单的处理，flask可以通过`from flask import request`后的`request.form`来获取来自post表单后的表单数据。这个应该基本满足你的需求，但如果你要写个什么后台管理系统，频繁的表单验证操作什么的一定会把搞的头晕脑胀的，这时试一试flask的外挂`flask-wtf`，能够很好地帮你解决表单一类常规事务。具体如何使用请移步到[wtf](https://wtforms.readthedocs.org/)官方文档

模板渲染的问题，flask采用`jinjia2`模板引擎，`jinjia2`有一套完整的模板语法规则，最常规的就是`{{ variable_name }}`这样的语句，当然除了变量替换外，`jinjia2`还可以进行一些逻辑判断、控制语句操作。这些可以完美的解决你web开发中的前端问题，此外《Flask Web开发：基于Python的Web应用开发实战》还推荐一个[Flask-Bootstrap]()插件扩展，帮助你更便捷的使用[twitter-bootstrap]()前端框架。

这里还需要额外说一下关于静态文件引用的处理方法。默认的情况Flask会自动生成一个`/static/<filename>`的路由，其对应的是应用目录中的`static`文件,可以通过`url_for('static', filename='css/styles.css', _external=True)`这样的语句，得到`http:// localhost:5000/static/css/styles.css`地址，该路由会到应用中的static文件去查找`style.css`文件，当然除了css样式之外，你也可以将js、图片等静态资源以这种方式进行引用

在前端方面，flask还带了一个我在rails中最喜欢的小功能，flash消息，使用其首先需要在应用中引入`flash`

`from flask import Flask,flash`


然后在控制器中对应的函数中调用`flash()`方法传入你需要显示的消息


`flash('Looks like you have changed your name!')`


最后在模板中根据样式需求渲染flash消息

```

{% for message in get_flashed_messages() %}
	{{ message }}
{% endfor %}

```

在数据库方面，flask并不对你使用何种数据库类型进行限制，可以采用诸如Mysql、Psoteres、SQLite、Redis等任意类型得数据库。而且不用考虑是否有插件支持的问题，即使原生Pyhton模块也能在flask里得到很好地支持。当然，如果有现成得ORM、ODM这样的抽象层在开发上肯定能更加便利。如你所想Flask确实有提供这样的插件——Flask-SQLAlchemy。使用方法如下
```
from flask.ext.sqlalchemy import SQLAlchemy
basedir = os.path.abspath(os.path.dirname(__file__))

app = Flask(__name__) app.config['SQLALCHEMY_DATABASE_URI'] =\
'sqlite:///' + os.path.join(basedir, 'data.sqlite')
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True

db = SQLAlchemy(app)
```

Mysql的连接方式`mysql://username:password@hostname/database?charset=utf-8`,留意末尾的字符集配置，这个设置不当可能会导致你后面取数据时出现乱码问题。

除了做好数据库之间的连接之外，还需建立数据模型，一个模型对应数据库中一张表，同时对应Python中的一个类。数据模型建立以后，就可以像使用Python对象一样去操作数据库中的内容了。

![SQLAlchemy 列类型]({{site.IMG_PATH/sqlaclchemy-column-type.png}})

在完成DB连接与模型创建之后，就可以像使用Python对象那样去操作数据库了。



请求调度方面有两个方法，一个是通过`@app.route('/',methods=['post','get'])`这样的装饰器去与对应的视图处理函数做匹配,另一个就是与django，tornado这样的框架一样集中的对路由做视图关联定义。后者比较适合大型项目集中管理，如果只是小项目个人认为还是默默的使用第一种就够了。


重点说一下**大型程序结构**这一部分，这部分本身没有什么特别的其实也都是架构在前面得基础之上的，但是通过阅读学习本章，可以让你更好去阅读github上别人的程序。也能让你学到更加工程化的做事方法

下图是一个Flask项目的基本目录结构

![Flask目录结构图]({{site.IMG_PATH}}/flask-directory-structure.png)

+ app 存放程序所有源代码 包含静态文件(static)、模板文件(templates)、控制器代码(main)以及一些库文件
+ migrations 存放数据库迁移文件
+ test 存放项目测试代码
+ venv virtualenv创建的虚拟运行环境
+ config.py 集中管理配置文件
+ manage.py 程序启动脚本，统一管理程序运行
+ requirements.txt  导出的项目所有依赖包，用于在其它环境中构建相同开发环境



### 书中提到得一些 工具
+ HTTPie 类似curl命令行工具，但比curl更简洁
+ coverage 检查代码覆盖工具,可以在任何python程序中使用 (pip install coverage)
+ Selenium  Web浏览器自动化工具 (pip install selenium)


### 书中推荐的插件列表
+ Flask-Moment 格式化显示时间
+ Flask-Bootstrap twitter-bootstrap 插件
+ Flask-Script 为flask添加一个命令行启动界面
+ Flask-WTF 表单插件，提供表单验证相关操作
+ Flask-SSLify 启用https
+ Flask-Mail 邮件插件
+ Flask-Babel 国际化
+ FLask-RESTful REST API 开发工具
+ Flask-DebugToolbar 浏览器中使用的调试工具
+ Frozen-Flask 把 Flask 程序转换成静态网站。
+ Flask-SQLAlchemy  数据库抽象层ORM
+ Flask-WhooshAlchemy  实现 Flask-SQLAlchemy 模型的全文搜索。
+ Flask-KVsession  服务器端存储用户会话


### 参考引用
+ [Flask扩展网站](http://flask.pocoo.org/extensions/)
+ [Python Package Index](http://pypi.python.org/)
