
---

title: django migrate报错

date: 2019-07-09 17:02:27 +0800

tags: []

---
<a name="iyBJN"></a>
# 背景
学习django过程中，执行python manage.py migrate报错，提示如下：

```python
django.db.utils.ProgrammingError: (1064, "You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '(6) NOT
 NULL)' at line 1")
```

<a name="5fVdB"></a>
# 过程
直接google，说是因为django2.1+版本需要mysql版本要5.6+，因此先判断下自己的版本<br />Django      2.2.3<br />mysql 5.5.4

django版本直接pip list查看，因为是pip安装；<br />mysql的版本的话，直接输入select version();

那么确定是版本问到导致的，解决方法，也提供了，如下：

```python
# 在项目的settings.py最上面添加如下代码
from django.db.backends.mysql.base import DatabaseWrapper
DatabaseWrapper.data_types['DateTimeField'] = 'datetime' # fix for MySQL 5.5
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/92887/1562663317661-fbf601fb-7bd9-4243-b077-dc37f6c32444.png#align=left&display=inline&height=386&name=image.png&originHeight=482&originWidth=1520&size=101977&status=done&width=1216)

