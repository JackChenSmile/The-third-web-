---
title: daythird
date: 2018-10-24 18:19:35
tags: 查、删、改
---

#### 查询表格中的数据（2）：

```python
from django.http import HttpResponse

1.或条件（引入函数模块Q）
# Q(),alt+enter ----- 引入模块的快捷键 ------- from django.db.models import Q, F
stus = Student.objects.filter(Q(s_age=20) | Q(s_gender=1))
2.且条件
stus = Student.objects.filter(Q(s_age=20) & Q(s_gender=1))
3.非条件
stus = Student.objects.filter(~Q(s_age=20))
4.查询表中数据的大小
stus = Student.objects.filter(字段__gt=F('另一个字段'))
5.查询语文比数学多10分的同学
stus = Student.objects.filter(yuwen__gt=F('math') + 10)

stu_names = [stu.s_name for stu in stus]    # 将stus这个字典变成一个生成器，取出每个同学的名字
return HttpResponse('查询成功')
```

#### 删除列表中的数据：

```python
def del_stu(request):
	# 实现删除
	Student.objects.filter(s_name='旺财').delete()
	return HttpResponse('删除成功')
```

#### 更新列表中的数据

```python
方法一：
def update_stu(request)：
	# 实现更新
	stu = Student.objects.filter(s_name'旺财').first()   # 取到的是一个字典，所以可以用first（）或者last（）
	# Student.objects.get(s_name='李哥')    # 从页面上获取s_name，也可以进行数据的更新
	stu.s_name = '王老五'
	stu.save()    # 保存更新后的信息
	return HttpResponse('修改成功')      # 返回一个提示语

方法二：
def update_stu(request):
    Student.objects.filter(s_name='被修改内容').update(s_name='修改内容')
    return HttpResponse('修改成功')
```

### 在页面上获取所有学生的信息

```python
from django.shortcuts import render

# 1.在文件中新建一个Directory,命名为templates，在templates文件中，新建一个HTML文件，就是网页文件名
# 2.在views.py中创建一个函数
def all_stu(request):
	# 获取所有学生的信息
    stus = Student.objects.all()
    # 返回页面
    return render(request, '页面文件名', 数据内容)

# 3.页面内容

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <table>
        <thead>
        <th>姓名</th>
        <th>年龄</th>
        <th>数学成绩</th>
        <th>语文成绩</th>
        <th>操作</th>
        </thead>
        {% for stu in students %}
        <tbody>
            <!--解析变量-->
            <td>{{ stu.s_name }}</td>
            <td>{{ stu.s_age }}</td>
            <td>{{ stu.math }}</td>
            <td>{{ stu.yuwen }}</td>
            <td>
                <a href="/add_info/?stu_id={{ stu.id }}">添加拓展信息</a>
            </td>
        </tbody>
        {% endfor %}
    </table>
</body>
</html>
# 4.在urls.py文件urlpatterns中添加访问的路由地址
# url(正则表达式，views.创建的函数名)
url(r'^all_stu/', views.all_stu)
```

### 一对一的搜索

```python
# 1.先创建一个一对一的类
class StudentInfo(models.Model):
    phone = models.CharField(max_length=11, null=True)
    address = models.CharField(max_length=100)
    # OneToOneField  指定一对一的关系
    stu = models.OneToOneField(Student)
    
    class Meat
        db_table = 'student_info'   # 对这个表格命名
# 2.创建一个函数
def add_info (request):
    # method 获取请求Http方式
    if request.method = 'GET':
        return render(request, 'info.html')
    if request.method = 'POST':
        # 获取页面中提交的手机号码和地址，并保存
        phone = request.POST.get('phone')
        address = request.POST.get('address')
        stu_id = request.GET.get('stu_id')
        # 保存
        StudentInfo.objects.create(phone=phone, address=address, stu_id=stu_id)
        return HttpResponse('创建拓展表成功')
# 3.在templates文件中创建一个info的HTML文件
# 内容：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <form action="" method="post">
        电话号码：<input type="text" name="phone">
        地址：<input type="text" name="address">
        <input type="submit" value="提交">
    </form>

</body>
</html>
# 4.设置路由地址
url(r'^sel_info/', views.sel_info)
```

### 一对多的搜索

```python
# 1.创建一个Grade的类
class Grade(models.Model):
    g_name = models.CharField(max_length=10, unique=True)

    class Meat:
        db_table = 'grade'
# 2.一个班级有多个学生，所以在学生类中添加主键
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
    # 定义yuwen字段，语文成绩
    yuwen = models.DecimalField(decimal_places=1, max_digits=4, null=True)
    #     # 定义math，数学成绩
    math = models.DecimalField(decimal_places=1, max_digits=4, null=True)

    grade = models.ForeignKey(Grade, null=True)      #  添加的主键
# 3.创建一个查询的函数
def sel_info_by_stu(request):
    if request.method == 'GET':
        # 通过学生查询拓展信息
        # 第一种方法
        stu = Student.objects.get(s_name='小明')
        info = StudentInfo.objects.filter(stu_id=stu.id)
        info = StudentInfo.objects.filter(stu=stu)
        
        # 第二种方法 ,学生对象，关联的模型名的小写（一对多）
        info = stu.studentinfo

        return HttpResponse('通过学生查找拓展表信息')
# 4.创建路由地址
url(r'^sel_info_by_stu/', views.sel_info_by_stu),

# 5.创建一个通过电话号码查找的函数

def sel_stu_by_info(request):
    if request.method == 'GET':
        # 知道手机号码，找人
        info = StudentInfo.objects.get(phone='12536524521')
        student = info.stu
        print(student)
        return HttpResponse('通过手机号码查找学生信息')

url(r'^sel_stu_by_info/', views.sel_stu_by_info),

# 6.通过班级查找
def add_grade(request):
    if request.method == 'GET':
        names = ['物联网', 'python', '外语']
        for name in names:
            Grade.objects.create(g_name=name)
        return HttpResponse('创建班级成功')
    

url(r'^add_grade/', views.add_grade),
    
    
def sel_stu_grade(request):
    if request.method == 'GET':
        # 通过学生查找班级
        stu = Student.objects.filter(s_name='小明').first()
        gradec = stu.grade
        # 通过班级查找学生
        grade = Grade.objects.get(g_name='物联网')
        student = grade.student_set.filter(s_gender=0).all()
        return HttpResponse('查询学生和班级信息')
    
url(r'^sel_stu_grade/', views.sel_stu_grade),


```

