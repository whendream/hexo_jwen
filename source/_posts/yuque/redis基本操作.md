
---

title: redis基本操作

date: 2019-04-16 20:16:48 +0800

tags: []

---
# 背景
业务版本中使用到了redis，需要验证数据存进redis是否正确

# 前言
Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。

# 基本操作
登录redis，两种方式都可以

1\. 使用telnet方式 

2\. 使用redis-cli命令进入

但都需要指定端口，查看redis占用了哪个端口

telnet 127.0.0.1 6380   

./redis-cli -p 6380

**根据key查看value**

get "key"

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555417003380-f96f6573-b429-426f-9c58-ca82786eacf6.png)

**删除key-value**

DEL "key"

更多请参考：http://www.runoob.com/redis/redis-tutorial.html
