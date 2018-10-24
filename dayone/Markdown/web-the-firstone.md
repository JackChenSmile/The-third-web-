---
title: web-the firstone
date: 2018-10-22 09:42:39
tags:
---

## 框架

### Django:Python Web应用开发框架

&emsp;&emsp;Django 应该是最出名的Python框架，GAE甚至Erlang都有框架受它影响。Django是走大而全的方向，它最出名的是其全自动化的管理后台：只需要使用起ORM，做简单的对象定义，它就能自动生成数据库结构、以及全功能的管理后台。

### Flask：一个用Python编写的轻量级Web应用框架

&emsp;&emsp;Flask是一个使用Python编写的轻量级Web应用框架。基于Werkzeug WSGI工具箱和Jinja2 
模板引擎。Flask也被称为“microframework”，因为它使用简单的核心，用extension增加其他功能。Flask没有默认使用的数
据库、窗体验证工具.

### Cubes：轻量级Python OLAP框架

&emsp;&emsp;Cubes是一个轻量级Python框架，包含OLAP、多维数据分析和浏览聚合数据（aggregated data）等工具.

### Pulsar：Python的事件驱动并发框架

&emsp;&emsp;Pulsar是一个事件驱动的并发框架，有了pulsar，你可以写出在不同进程或线程中运行一个或多个活动的异步服务器.

### Web2py：全栈式Web框架

&emsp;&emsp;Web2py是一个为Python语言提供的全功能Web应用框架，旨在敏捷快速的开发Web应用，具有快速、安全以及可移植的数据库驱动的应用，兼容Google App Engine.

### Scrapy：Python的爬虫框架

&emsp;&emsp;Scrapy是一个使用Python编写的，轻量级的，简单轻巧，并且使用起来非常的方便.

## Django

### MVC：

M(模型层)、V(视图层)、C(业务层)

&emsp;&emsp;MVC（Model View Controller）是模型-视图-控制器，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑狙击到一个部件里，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。通俗的来讲就是，强制性的使应用程序的输入，处理和输出分开。

![图示](https://github.com/JackChenSmile/The-third-web-/blob/master/dayone/Markdown/web-the-firstone/QQ%E6%88%AA%E5%9B%BE20181022103251.png)

### MVT：

M(模型层)、V(视图：处理业务逻辑)、T(模板Template：html)

&emsp;&emsp;严格来说，Django的模式应该是MVT模式，本质上和MVC没什么区别，也是各组件之间为了保持松耦合关系，只是定义上有些许不同。

Model： 负责业务与数据库(ORM)的对象

View： 负责业务逻辑并适当调用Model和Template

Template: 负责把页面渲染展示给用户

注意： Django中还有一个url分发器，也叫作路由。主要用于将url请求发送给不同的View处理，View在进行相关的业务逻辑处理。

## 安装Django和创建Django的虚拟环境

### 创建虚拟环境（1）

#### 操作命令

##### VIRTUALENV创建虚拟环境

1.在cmd中能通过python去启动，如果不可以，直接跳到第三部

![python](https://github.com/JackChenSmile/web-the-firstone/blob/master/Markdown/web-the-firstone/python.png)

2.在cmd中能通过pip3启动安装软件，如果不可以直接跳到第三步

![pip3](https://github.com/JackChenSmile/web-the-firstone/blob/master/Markdown/web-the-firstone/pip3.png)

3.配置python环境和pip环境

![python_pip_envir](https://github.com/JackChenSmile/web-the-firstone/blob/master/Markdown/web-the-firstone/python_pip_envir.png)

4.确认pip安装成功，如果Scrip文件夹下没有pip可执行文件，则执行第五步

5.由于python3.6安装以后，在Scrip文件中没有pip的可执行软件，需要输入下一个命令进行安装

`python -m ensurepip`

![图片](https://github.com/JackChenSmile/web-the-firstone/blob/master/Markdown/web-the-firstone/ensurepip.png)

注：现在在python的安装文件夹Scripts下就有pip.exe以及easy_install.exe等可执行文件了，就可以使用pip安装

##### window中安装使用

1.安装virtualenv

`pip install virtualenv`

![pip_virtualenv](https://github.com/JackChenSmile/web-the-firstone/blob/master/Markdown/web-the-firstone/pip_virtualenv.png)

2.创建虚拟环境

先查看一下安装虚拟环境有那些参数，是必须填写的

![virtualenv_help](https://github.com/JackChenSmile/web-the-firstone/blob/master/Markdown/web-the-firstone/virtualenv_help.png)

参数： --no-site-package 和-p参数

```
创建纯净的虚拟环境：
virtualenv --no-site-package  环境名
```

以下是指定安装虚拟环境中的python版本和安装方式：

python版本3.6 + Django 1.11

![virtualenv_env_p](https://github.com/JackChenSmile/web-the-firstone/blob/master/Markdown/web-the-firstone/virtualenv_env_p.png)

3.进入/退出evn

```
进入 cd env/Scripts/文件夹 在activate命令
退出 deactivate
```

ubuntu中安装使用

1.安装virtualenv

`apt-get install python-virtualenv`

2.创建包含python3版本的虚拟环境

`virtualenv -p /usr/bin/python3 env`

env代表创建的虚拟环境的名称

3.进入/退出env

```
进入 source env/bin/activate
	pip list
	pip install django==1.11 ---------- 安装django
	pip install PyMySQL ------------ 安装PyMySQL
退出 deactivate
```

4.pip使用

查看虚拟环境下安装的所有包

`pip list`

查看虚拟环境中通过pip安装的包

`pip freeze`

#### 创建虚拟环境（2）

1.新建文件夹  -------->  env

2.在env中 新建文件 --------- djenv6

3.在djenv6中，保存pip所有需要的文件

4.通过`pip list`查看虚拟环境下安装的所有包

5.安装Django

`pip install django==1.11`

## 在Django的虚拟环境中

- 创建项目

  `django-admin startproject day01`

- 在python中打开文件

  ![图片](https://github.com/JackChenSmile/The-third-web-/blob/master/dayone/Markdown/web-the-firstone/QQ%E6%88%AA%E5%9B%BE20181022171438.png)

  - __init__.py：说明该目录结构是一个python包，会在后期被用来初始化一些工具。
  - seetings.py：Django项目的配置文件，其中定义了本项目的引用组件，项目名，数据库，静态资源，调试模式，域名限制等，可以用来修改一些文件设置。
  - urls：项目的URL路由映射，实现客户端请求的url由那个模块进行响应。
  - wsgi.py：定义WSGI接口信息，通常本文件生成后无需改动。
  - manage.py：是Django用于管理本项目的管理集工具，之后站点运行，数据库自动生成，数据表的修改等都是通过该文件完成。

- Django驱动页面

  `python manage.py runserver`

- 修改Django的驱动页面的语言设置

中文和时区设置：

![图片](https://github.com/JackChenSmile/The-third-web-/blob/master/dayone/Markdown/web-the-firstone/QQ%E6%88%AA%E5%9B%BE20181022152640.png)

英文和时区设置：

![图片](https://github.com/JackChenSmile/The-third-web-/blob/master/dayone/Markdown/web-the-firstone/QQ%E6%88%AA%E5%9B%BE20181022152916.png)

- 启动Django的方法

  `python manage.py runserver 8080`

  `python manage.py runserver IP:8080`

- 访问管理后台 ------- admin

  `http://127.0.0.1:8080/admin`

- 修改数据库的配置 -------- settings.py

  `ENGINE, USER, PASSWORD, HOST, PORT, NAME`

- 将Django中自带的模型迁移（映射）到数据库中

  `python manage.py migrate`

- 安装数据库驱动

  `pip install pymysql`

- 初始化数据库的驱动______init__.py

  ```python
  import pymysql
  pymysql.install__as__mysqldb()
  ```

- 设置Django后台账户和密码

  ```python
  python manage.py createsuperuser
  python manage.py changepassword
  ```
