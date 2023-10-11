---
layout:     post
title:      "django.db.utils.OperationalError: (1050, Table '表名' already exists"
date:       2022-11-04 22:51:00
tag:        django
description: ""
---
解决方法：

`python manage.py migrate [myapp] --fake`

数据库表字段变更比较频繁。models.py表类中添加了一个class类后。执行manage.py makemigrations 未提示错误信息，但manage.py migrate时进行同步数据库时出现问题;django.db.utils.OperationalError: (1050, "Table '表名' already exists）错误信息

[django.db.utils.OperationalError: (1050, "Table '表名' already exists）解决方法](https://www.javazxz.com/thread-11494-1-1.html)
