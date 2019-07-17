
---

title: redis 多实例

date: 2019-06-19 21:53:24 +0800

tags: []

---
<a name="8maLG"></a>
# 背景
业务侧需要起一个新的redis实例来存储，避免干扰到现有的实例数据，现在有的redis实例是6379端口，还有一个6380端这个是redis从库的

<a name="6LbwC"></a>
# 操作

1. 查看redis的help文档，你会发现如下：

![image.png](https://cdn.nlark.com/yuque/0/2019/png/92887/1560952631181-c3ebe187-166e-487a-9e7d-190ecc1acac7.png#align=left&display=inline&height=341&name=image.png&originHeight=341&originWidth=879&size=40442&status=done&width=879)<br />

2. 我们可以启动指定conf文件的形式，接下来我们查看下现有的6379.conf（一般的目录为/etc/redis/）

我们需要修改的地方：

```
port 6379
daemonize no
pidfile /var/run/redis_6379.pid
logfile /data/redis/log/redis.log
dbfilename dump.rdb
dir /data/redis/data
```
根据字段名很好的理解，其中要注意的就是daemonize 这里需要把no修改为yes，意思是以守护进程启动，就是可以后台运行；其他可以直接替换6379位6380即可（假设新的实例端口为6380）<br />

3. 启动实例，redis-server /etc/redis/redis_6380.conf就可以了
3. 验证，ps -ef | grep redis

<a name="BFZYv"></a>
# 疑问
其中有个slaveof的参数。如上图的操作 ./redis-server --port 7777 --slaveof 127.0.0.1 8888 ，本地起了一个7777端口的实例，这个实例的是本地8888端口实例的从库，如果8888端口实例有更新，7777端口的实例也会同步更新

