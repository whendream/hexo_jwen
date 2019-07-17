
---

title: python安装mysql-python依赖包

date: 2019-04-16 15:52:53 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景
新公司，对换工作了！接口自动化使用的是python的behave框架，因此需要折腾python了，而公司配的笔记本是windows的，因此要在windows下折腾python了

<a name="52b36576"></a>
# 步骤
项目中使用的setup.py文件来管理依赖的，通过ide直接安装依赖的时候提供mysql-python安装失败，如下

MySQLdb/_mysql.c(29) : fatal error C1083: Cannot open include file: 'mysql.h': No such file or directory

还有其他的各种错误，一顿google最后还是解决了

1. 安装wheel，通过pip install wheel安装即可

2. 安装whl包，这个包从[https://link.jianshu.com/?t=http://www.lfd.uci.edu/~gohlke/pythonlibs/](https://link.jianshu.com/?t=http://www.lfd.uci.edu/~gohlke/pythonlibs/)去获取，

![](https://img2018.cnblogs.com/blog/411616/201904/411616-20190408135406204-3297367.png#align=left&display=inline&height=128&originHeight=128&originWidth=705&status=done&width=705)

下载相应的版本，然后通过pip install 安装下载好的whl文件

3. 然后再执行pip install mysql-python

<a name="7a0a21d5"></a>
# 后记
是经过多次尝试后，成功了，其中也安装过vcforpython，说是因为windows缺少编译组件，如果上面步骤不成功，可以尝试安装下这个编译环境

