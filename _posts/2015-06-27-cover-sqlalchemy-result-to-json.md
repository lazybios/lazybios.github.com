---
layout: post
title: "Sqlalchemy查询结果直接转json"
date: "2015-06-27 13:37"
---

Sqlalchemy是一款很好用的Python ORM 工具, 配合flask可以快速构建出Restful api，不过美中不足就是Sqlalchemy的查询结果不能直接通过jsonify函数直接转换。一种变通的办法就是通过在
在创建数据类的时候，就实现一个`to_json`的方法，代码如下:

```python

class Article(Base):
    __tablename__ = 'article'
    id = Column(Integer, primary_key=True)
    title = Column(String(250), nullable=False)
    link = Column(String(255), nullable=True)
    cover = Column(String(255), nullable=False)
    nickname = Column(String(255), nullable=False)
    content = Column(Text(), nullable=False)

    def to_json(self):
        return {
                'id': self.id,
                'title': self.title,
                'link': self.link,
                'cover': self.cover,
                'nickname': self.nickname,
                'content' : self.content
                }

```

`to_json`方法返回一个字典类型，在最终生成json数据的时候，可以提前调用查询结果的类方法`to_json`拼接出任意需求的数据格式,将拼接后的结果数据传入到`jsonify`中。

```python

res =  db.session.query(Article).all()
temp = []
for x in res:
    temp.append(x.to_json())

jsonify(objects = temp)

```
