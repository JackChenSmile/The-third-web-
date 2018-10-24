---
title: web-daytwo
date: 2018-10-23 10:40:40
tags: web框架
---

#### 进入虚拟环境
- env/djenv6/Scripts/activate
  - 将代码放到code文件中,从虚拟环境中，进入code文件夹
- 退出虚拟环境 ------- deactivate

#### 启动一个脚本文件：

![QQ截图20181023121317](web-daytwo\QQ截图20181023121317.png)

- Run -> Debug Configurations ---- 修改

  - Parameters 改成 runserver 8080

- 引入Django模块

- 修改settings中的DATABASE的数据

  ![QQ截图20181023141556](web-daytwo\QQ截图20181023141556.png)

#### 在model中创建表格模型
1.在settings中的INSTALLED_APPS中添加app

![QQ截图20181023121548](web-daytwo\QQ截图20181023121548.png)

2.执行python manage.py startapp app

3.执行python manage.py makemiigrations

-  Provide a one-off default now (will be set on all existing rows) --------- 缺少一个默认值

4.再执行python manage.py migrate  ---- 迁移表格

注意：如果执行后没有生成迁移文件，一直提示**No changes detected**这个结果的话，就要手动的去处理了

- 处理方式一：

  - 先删除_pycache__文件夹
  - 直接强制的去执行迁移命令，python manage.py makemigrations xxx (xxx就是app的名称)
  - 查看自动生成的数据库，查看表django_migrations,删掉app字段为xxx的数据（xxx就是app的名称）

- 处理方式二

  - 执行迁移生成数据库

    **`python manage.py migrate`**

    注意：生成迁移文件的时候，并没有在数据库中生成对应的表，而是执行migrate命令之后才会在数据库中生成表

5.如果报错，查看Django_migrations的表格的最后一行

#### 表格中的操作
- 添加表格的列（定义字段）都写在models.py中创建的模型（类）中，定义字段，就表示定义这个类的对象的属性，然后执行上面的第三、四步骤就创建好了

```python
  from django.db import models
  
  # Create your models here.
  class Student(models.Model):
      # 定义s_name字段，varchar类型，最长不超过6字符，唯一
      s_name = models.CharField(max_length=6, unique=True)
      # 定义s_age字段，int类型
      s_age = models.IntegerField(default=18)
      # 定义s_gender字段，bool型
      s_gender = models.BooleanField(default=1)
      # 定义create_time字段，创建时间
      create_time = models.DateTimeField(auto_now_add=True, null=True)
      # 定义operate_time字段，修改时间
      operate_time = models.DateTimeField(auto_now=True, null=True)
  
  
      class Meta:
          # 定义模型迁移到数据库中的表格
          db_table = 'student'
```

##### （增）创建表格中的数据（1）：

- **1.url路由**

  在urls.py --> urlpatterns中添加一个url（正则表达式，调用views中的方法）

```python
    from app import views
    
    urlpatterns = [
        # 127.0.0.1:8080/admin/
        url(r'^admin/', admin.site.urls),
        # 127.0.0.1/8080/hello/
        url(r'^hello/', views.hello),
        # 127.0.0.1:8080/create_stu/
        url(r'^create_stu', views.create_stu)
```

```python
    def hello(request):
        return HttpResponse('你好， hello！')
    
    
    def create_stu(request):
        # 创建学生
        stu = Student()
        stu.s_name = '李二狗'
        stu.s_age = 20
        stu.save()
        return HttpResponse('创建成功')
```

   - **2.运行程序**

    查看调用方法生成的ip地址，在数据库中就创建好了数据
    
    ![QQ截图20181023122235](web-daytwo\QQ截图20181023122235.png)

##### 创建表格中的数据（2）

```python
def create_stu(request):
    Student.objects.create(s_name='小明')
    return HttpResponse('创建成功')
```

##### （查）查询数据 ：

**准确查询**

```python
def sel_stu(request):
    # 实现查询
    # 查询所有的数据
    stus = Student.objects.all()
    # filter() 过滤
    stus = Student.objects.filter(s_name='小明')
    # first()获取第一个
    # last()获取最后一个
    stus = Student.objects.filter(s_age=20).first()
    # get拿不到值和拿到多个值都会报错，filter不会报错，会取到空值
    stus = Student.objects.get(s_age=20)
    # 查询多个条件
    stus = Student.objects.filter(s_age=20).filter(s_gender=1)
    stus = Student.objects.filter(s_age=20, s_gender=1)
    print(stu_names)
    return HttpResponse('查询成功')
```

**模糊查询**

```python
def sel_stu(request):
# 模糊查询 like '%xxx%' '_xx'
	# 名字中含有'吴'字
    stus = Student.objects.filter(s_name__contains='吴')
    # 以小开头
    stus = Student.objects.filter(s_name__startswith='小')
    # 以小字结尾
    stus = Student.objects.filter(s_name__endswith='小')
    stu_names = [stu.s_name for stu in stus]
    print(stu_names)
    return HttpResponse('查询成功')
```

**其它查询**

```python
def sel_stu(request):
	# 大于 gt/gte  小于 lt/lte
    stus = Student.objects.filter(s_age__gt=18)
    stus = Student.objects.filter(s_age__lt=18)
    stus = Student.objects.filter(s_age__gte=18)

    # 排序 order_by()
    # 升序
    stus = Student.objects.order_by('id')
    # 降序
    stus = Student.objects.order_by('-id')

    # 查询部满足条件的数据
    stus = Student.objects.exclude(s_age=20)

    # 计数方法count(),len()
    print(len(stus))
    stus_count = stus.count()
    print(stus_count)

    # values()
    stus_values = stus.values()

    # id=pk
    stus = Student.objects.filter(id=2)
    stus = Student.objects.filter(pk=2)

    stu_names = [stu.s_name for stu in stus]
    print(stu_names)
    return HttpResponse('查询成功')
```

