
---

title: jvm linux 时区设置

date: 2019-04-17 19:55:43 +0800

tags: []

---
\# 背景
=====

在接入集团一个平台的时候，发现录制某个接口到测试环境回放，发现接口入参一致，一个start\_day 一个end\_day，但回放的时候会多调用一次数据库查询，很是奇怪；

查阅业务代码，发现确实有逻辑会导致多查询一次，于是重点观察数据变化，发现录制回放两个时间不一致，相差12个小时；

继续查阅业务日志，发现在第一次查询DB的时候，两次的时间不一样，就是说接口入参（String类型）一致，通过应用转化为int类型的时候就出问题了的，相差12个小时；

因此猜测是**时区问题！！！**

\# 思路&解决
========

1\. 既然发现是时区问题，比较好搞咯，去到录制机器A和回放机器B，通过linux命令查看时区

date -R  
发现都是Fri, 06 Jul 2018 12:11:22 +0800  
都是+8,东八区  
  
date +"%Z %z"  
结果发现还是一致呀，都是CST +0800

2\. 不对，时区一样呀，那么问题就是java执行不一样？ 核对了jdk版本，发现一致

3\. 那么就在两台机器上执行java代码试下：

System.out.println(TimeZone.getDefault()); //输出当前默认时区

发现了问题了，两台机器打印的不一致，A是上海，而B是纽约。。。

4\. 那么问题变成了jvm从哪里去获取时区的呢？经过查询大致如下：

> 1)如有环境变量 TZ设置，则用TZ中设置的时区
> 
> 2) 在 /etc/sysconfig/clock文件中找 "ZONE"的值
> 
> 3）如2)都没，就用/etc/localtime 和 /usr/share/zoneinfo 下的时区文件进行匹配，如找到匹配的，就返回对应的路径和文件名。

简单来说就是:

TZ环境变量 --> /etc/sysconfig/clock文件  --> /etc/localtime文件 依次寻找

5\. 于是开始设置了，TZ不管了，加了/etc/sysconfig/clock，如下操作：

新建一个/etc/sysconfig/clock，内容如下：

ZONE="Asia/Shanghai" UTC\=false ARC\=false

然后继续去查看时区，还是不对呀！！

6\. 继续设置/etc/localtime文件，如下操作：

unlink /etc/localtime 
ln \-s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

就是初始化/etc/localtime ，然后将东八区的绑定上

7\. 在查看时区成功了，重新执行java代码，发现正常了

8\. 继续翻阅资料，发现：

> 时区的配置文件是/etc/sysconfig/clock。用tzselect命令就可以修改这个配置文件，根据命令的提示进行修改就好了。
> 
> 但是在实际工作中，发现这种方式是不能够使得服务器上的时间设置马上生效的，而且使用ntpdate去同步时间服务器也不能够更改时间。即使你使用了 date命令手工设置了时间的话，如果使用ntpdate去进行时间同步的话，时间又会被改动到原来的错误时区的时间。而生产的机器往往是非常重要的，不能够进行重启等操作。
> 
> 1）/etc/sysconfig/clock 文件，只对 hwclock   
> 命令有效，且只在系统启动和关闭的时候才有用（修改了其中的 UTC=true 到 UTC=false 的前后，执行 hwclock (--utc,  
> 或 --localtime) 都没有变化，要重启系统后才生效）；  
> 在 /etc/sysconfig/clock 中 UTC=false 时，date、hwclock、hwclcok --localtime 输出的时间应该都一致，且此时 hwclock --utc是没有意义的；  
>   
> 在 /etc/sysconfig/clock 中 UTC=ture 时，date、hwclock 的输出是一致的，hwclock --localtime 的输出则是UTC时间；  
> 系统关闭时会同步系统时间到硬件时钟，系统启动时会从硬件时钟读取时间更新到系统，这2个步骤都要根据 /etc/sysconfig/clock 文件中UTC的参数来设置时区转换。

意思就是修改/etc/sysconfi/clock是可行的，但是不会立即生效，需要重启。那么一切就说的通了

\# 后记
=====

参考资料：

https://www.nowcoder.com/questionTerminal/1e794493ad564324a16da1c47545c117

http://blog.51cto.com/5iwww/661863

https://my.oschina.net/huawu/blog/4646

http://linux.it.net.cn/CentOS/fast/2016/0511/21660.html

https://blog.csdn.net/splenday/article/details/47065557

https://unix.stackexchange.com/questions/110522/timezone-setting-in-linux
