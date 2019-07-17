
---

title: python- 动态加载目录下所有的类

date: 2019-04-16 15:38:01 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景

自动化测试框架中model层下有很多类，用来操作mysql的，使用的时候需要把全部的类加载进来，需要使用到动态加载类

<a name="957a228f"></a>
# 解决方法

使用pkgutil，内置的方法，常用的话有两个方法<br />iter_modules(path=None, prefix='')<br />Yields (module_loader, name, ispkg) for all submodules on path, or, if path is None, all top-level modules on sys.path.<br />path是包的目录路径，prefix是输出时，所有包的名字的前缀。用来获取该path下的子模块或子包。

walk_packages(path=None, prefix='', onerror=None)<br />Yields (module_loader, name, ispkg) for all modules recursively on path, or, if path is None, all accessible modules.<br />同上，但是这个方法是递归获取路径下的所有模块。<br />具体使用如下：

```python
# 动态加载modelsql中所有类
for importer_sql, modname, ispkg_sql in pkgutil.walk_packages(path=modelsql.path,

prefix=modelsql.name+'.',

onerror=lambda x: None):

exec('from ' + modname + ' import *')
```

相当于对目录下所有的类执行了import *的操作


