---
title: django外键之指定字段
tags:
  - django
categories: 技术
abbrlink: b31a5b38
date: 2019-08-01 00:00:00
---
## 背景

* 数据库表已经定义，有关联关系字段
* django程序model指定关联关系字段  
<!-- more -->

## 解决方法  

### 通用外键   

```python
from django.db import models

class Car(models.Model):
    manufacturer = models.ForeignKey(
        'Manufacturer',
        on_delete=models.CASCADE,
    )
    # ...

class Manufacturer(models.Model):
    # ...
    pass
```
指定外键  
```python
from django.db import models

class User(models.Model):
	user_name = models.CharField(max_length=32)
	group = models.ForeignKey(
        	  'Group',
        	  to_field='group_id',  # 关联model的字段
       	      db_column='group_num',  # 自身表对应字段
        	  verbose_name='用户组',
        	  on_delete=models.CASCADE)

	 
class Group(models.Model):
	group_id = models.CharField(max_length=32)
	group_name = models.CharField(max_length=32)
    
```

##  运用场景
### 通用外键
* 数据库表的创建使用django通用的 %s_id 作为外键 如：group_id
* 未创建数据库，使用django统一创建

### 指定外键  

* 数据库表的创建使用非django 通用的字段作为外键 如：group_num
* 已创建数据库，指定关联字段


## 感谢  

* [官方文档](https://docs.djangoproject.com/en/2.2/ref/models/fields/#django.db.models.ForeignKey)   

* [django源码](https://github.com/django/django/blob/master/django/db/models/fields/related.py)  
