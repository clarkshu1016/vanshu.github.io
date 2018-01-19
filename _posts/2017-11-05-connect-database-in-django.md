---
layout: post
title: Django的常用配置
date: 2017-11-02
categories: [Python]
tags: [python]
description:
icon: icon-html

---
##  Django的常用配置

* 如何生成django默认表结构
    1. 运行Tools-Run manager.py Task
    2. 输入makemigrations
    3. 输入migrate 生成数据表结构
    
    
* 数据库连接配置，在settings.py文件当中配置如下:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        # 'NAME': os.path.join(BASE_DIR, 'mxonline'),
        'NAME': 'mxonline',
        'USER': 'root',
        'PASSWORD': 'root',
        'HOST': '127.0.0.1',
    }
}
```
