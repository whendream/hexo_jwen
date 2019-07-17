
---

title: ip网段变更

date: 2019-04-16 19:42:38 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景

公司网络跟集团靠拢，先走第一步：IP网段变更。从XX网段切换到OO网段

<a name="ea340b9d"></a>
# 方法

1. 准备工作
  1. 保证IPMI连接正常
  1. 获得新IP并核对对应主机名、旧IP是否相符

2. 确认网卡名称

#找到目前配置旧业务IP的活动网卡,如eth0，以各机器实际使用网卡为准,本文以eth0为例

ifconfig

3. 修改配置文件

#修改网卡配置文件

vim /etc/sysconfig/network-scripts/ifcfg-eth0

GATEWAY="111.11.1.1" #修改为分配的新网关

IPADDR="111.11.1.8" #修改为分配的新IP

NETMASK="255.255.255.55" #修改为分配的新掩码

#修改网络配置文件

vim /etc/sysconfig/network

GATEWAY=111.11.1.1 #修改为分配的新网关

4. 重启网络服务

service network restart

5. 修改DNS服务IP（如有需要）

#cat /etc/resolv.conf<br />
nameserver 新ip<br />
nameserver 新ip

