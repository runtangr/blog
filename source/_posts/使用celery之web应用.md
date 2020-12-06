---
title: 使用celery之web应用
tags:
  - celery
  - flask
categories: 技术
abbrlink: 4bd0c7ac
date: 2018-10-26 00:00:00
---
## 起源
 * web 应用采用同步方式，响应时间过长。
 * 现有web项目基于flask, celery异步任务功能健全, 方便, 更加适合。

## 处理流程
![](/images/celery_web/celery_web_1.jpg)

## demo
local.py
```python
# -*- coding:utf-8 -*-

from flask_script import Manager
from flask import request
from make_celery import make_celery
from config import config

app = Flask(__name__)
app.config.from_object(config['local'])
config['local'].init_app(app)
app.config.update(RESTFUL_JSON=dict(ensure_ascii=False))

from celery_task import celery_test

# 初始化celery
celery = make_celery(app)
manager = Manager(app=app)

@app.route('/', methods=['GET', 'POST'])
def index():
    test = request.args.get('test','')
    msg = {'test': test}
    result = celery_test.delay(msg)
    return ','.join(['status:{}'.format(result.status),
                     'task_id:{}'.format(result.task_id)
                     ])
             
                     
if __name__ == '__main__':
    manager.run()
```
config.py
```python
# -*- coding:utf-8 -*-


class Config:
    
    @staticmethod
    def init_app(app):
        pass


class LocalConfig(Config):
    CONFIG_NAME = 'local'
   
    # CELERY
    CELERY_BROKER_URL = 'redis://localhost:6379/1'
    CELERY_RESULT_BACKEND = 'redis://localhost:6379/2'
    CELERY_TIMEZONE = 'Asia/Shanghai'
    CELERY_IMPORTS = (
        "celery_task",
    )

config = {
    'local': LocalConfig,
}

```
make_celery.py
```python
# -*- encoding: utf-8 -*-
from celery import Celery

celery = None


def make_celery(app):
    global celery
    celery = Celery(app.import_name, backend=app.config['CELERY_RESULT_BACKEND'], broker=app.config['CELERY_BROKER_URL'])
    celery.conf.update(app.config)

    class ContextTask(celery.Task):
        def __call__(self, *args, **kwargs):
            with app.app_context():
                return self.run(*args, **kwargs)

    celery.Task = ContextTask
    return celery
```
celery_task.py
```python
from make_celery import celery


@celery.task()
def celery_test(msg):
    # deal msg
    print(msg)
    return 'balabala'
```
## 启动
python local.py runserver  
celery worker -A local.celery -l info  
访问: http://127.0.0.1:5000/
## 思考和深入
    * 学习基础用法.
    * 学习高级用法.
    * 学习相关依赖库.