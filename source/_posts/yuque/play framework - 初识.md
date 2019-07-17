
---

title: play framework - 初识

date: 2019-04-16 16:36:42 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景

研发代码框架是play-framework框架，想看代码的话，需要学习下play框架。IDE工具的话之前一直用的idea，所以本文涉及的idea play的配置 和 一些play的简单知识

<a name="3fb2f32d"></a>
# 认识play

百度百科如下：

> play framework是一个full-stack（全栈的）Java Web的应用框架，包括一个简单的无状态MVC模型，具有Hibernate的对象持续，一个基于Groovy的[模板引擎](https://baike.baidu.com/item/%E6%A8%A1%E6%9D%BF%E5%BC%95%E6%93%8E/907667)，以及建立一个现代Web应用所需的所有东西。


前提是安装jdk，play也分两个大的版本，1.X和2.X，跟着我们这版研发版本走，使用的1.4.4版本

<a name="dbabf722"></a>
## play安装

使用的是mac，理论上可以支持brew安装的，但我期望的安装低版本的，直接下载bin包来配置。

1. 下载
<br />play的下载地址：[https://www.playframework.com/releases](https://www.playframework.com/releases)
<br />选择下载自己期望的版本
2. 配置环境变量
<br />配置一个play的home目录即可，添加到path中，如下：<br />
![](https://img2018.cnblogs.com/blog/411616/201812/411616-20181215172752409-994205794.png#align=left&display=inline&height=146&originHeight=218&originWidth=1076&status=done&width=720)
3. 测试
<br />配置完记得source下，直接执行play，就可以看到效果<br />
![](https://img2018.cnblogs.com/blog/411616/201812/411616-20181215172807313-1837231567.png#align=left&display=inline&height=411&originHeight=562&originWidth=984&status=done&width=720)

<a name="a4e6c251"></a>
## play-framework 依赖管理

之前熟悉了maven来管理jar包的依赖，play是通过dependencies.yml文件来管理依赖的，直接执行play dependencies命令的话，就会更新下载依赖，目前只要掌握这个命令即可

可以直接参考https://blog.csdn.net/twx843571091/article/details/50037393

<a name="4929ab60"></a>
## idea配置

idea支持1.X版本的play了的，不需要额外配置，但是要简单执行一个命令，进入到项目目录，执行`play idea`会生成一个ipr文件，然后idea打开这个ipr文件即可

![](https://img2018.cnblogs.com/blog/411616/201812/411616-20181215172820837-378201831.png#align=left&display=inline&height=434&originHeight=520&originWidth=894&status=done&width=746)

有play的jar包和playFramework Dependencies表示是play项目

<a name="12f1d7ef"></a>
# 结束
历史原因选择了play框架，知道后续新的应用都是走的spring boot。。。

