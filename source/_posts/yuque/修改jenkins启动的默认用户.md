
---

title: 修改jenkins启动的默认用户

date: 2019-04-17 19:56:38 +0800

tags: []

---
\# 背景
=====

通过yum命令安装的jenkins，通过service jenkins去启动jenkins的话，默认的用户是jenkins，但jenkins这个用户是无法通过su切换过去的 ，在某些环节可能产生问题，期望修改默认启动用户

\# 过程
=====

1\. 先修改/etc/sysconfig/jenkins文件中的参数，JENKINS\_USER

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555502187913-44f286e4-a6cf-44c1-a998-db25176034e4.png)

2\. 修改jenkins启动涉及到的目录权限，修改为nemo

目录如下：

/var/lib/jenkins/
/var/log/jenkins/
/var/cache/jenkins/
/usr/lib/jenkins/jenkins.war /etc/sysconfig/jenkins

修改命令如下：

chown -R nemo:nemo 目录

注：nemo是一个用户名字，修改为期望的用户即可
