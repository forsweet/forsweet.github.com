title: "Django入门"
tags:[python,django,web]
---
#简介

Django提供MVC模式的框架
[Django中文教程][]
[Django英文教程][]
#安装
略

#新建一个Django项目
安装完毕后

`$ django-admin startproject mysite`

得到如下目录结构
<A ID="目录结构"> </A>
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
#Django配置
##配置数据库
设置`mysite/settings.py` 文件中`DATABASES`变量的`default`字段的各个值:

* 默认使用的数据库为sqlite
* 各字段含义:
	* ENGINE -各种数据库的类型 `django.db.backends.sqlite3`, `django.db.backends.postgresql_psycopg2`, `django.db.backends.mysql`, 或 `django.db.backends.oracle`
	* NAME -数据库实例名
	* 若非sqlite数据库,USER,PASSWORD,HOST也需要被添加
* 更多设置参见[DATABASES][]

##配置时区

设置`mysite/settings.py` 文件中的`TIME_ZONE`

##配置默认应用

设置`mysite/settings.py` 文件中的`INSTALLED_APPS`
这里保存了当前Django项目中被激活的Django应用的名字.

默认安装了如下应用:
* django.contrib.admin－管理界面。你将会在教程的第二部分使用到它。
* django.contrib.auth－一个身份认证系统。
* django.contrib.contenttypes－一个内容类型框架。
* django.contrib.sessions－一个会话框架。
* django.contrib.messages－一个消息框架。
* django.contrib.staticfiles－一个用来管理静态文件的框架

以上应用在被使用时需要先初始化数据库表:
`$ python manage.py migrate`
manage.py位于生成的文件夹中,见[目录结构](#目录结构)
#开发服务器
该服务器仅用于开发调试,用如下命令启动
`$ python manage.py runserver`
启动成功日志:
```bash
Performing system checks...

0 errors found
September 03, 2014 - 15:50:53
Django version 1.7, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
之后打开http://127.0.0.1:8000/ 就可以看到标题为It worked! 的欢迎页面

* 启动命令也可以用如下格式:
`$ python manage.py runserver [ip_host] [port]`
这样就可用http://[ip_host]:[port]/来打开页面

* runserver能够根据文件变更自动重新载入python代码,但是某些行为,如添加文件时,不会触发自动重新载入,所以此时需要手工重启服务器
*  更多runserver参见[文档][runserver]

#创建模型
在`manage.py`同层运行如下命令:
`$python manage.py startapp polls`
Django可以自动初始化一个叫polls的应用,目录结构如下:
```
polls/
    __init__.py
    admin.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```


[Django中文教程]: http://django-1-7-doc.coding.io/index.html
[Django英文教程]: https://docs.djangoproject.com/en/1.8/
[DATABASES]: https://docs.djangoproject.com/en/1.8/ref/settings/#std:setting-DATABASES
[runserver]: https://docs.djangoproject.com/en/1.8/ref/django-admin/#django-admin-runserver