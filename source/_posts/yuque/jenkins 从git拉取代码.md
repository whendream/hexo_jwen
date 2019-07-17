
---

title: jenkins 从git拉取代码

date: 2019-04-17 20:18:27 +0800

tags: []

---
步骤
==

jenkins已集成git插件（如无，请自行下载）

1\. 去到源码管理栏，选中Git：
------------------

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503497725-35e4cd01-7345-47d7-94f9-4f024b9f643b.png)

 **使用http协议去获取代码**
------------------

 Repository URL填写http的git地址，此时未选择相应的Credentials，会有图中红色提示

****![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503497864-25f199fd-1f13-4dff-be08-ffcb24c37195.png)****
-----------------------------------------------------------------------------------------------------

 HTTP协议的话，需要输入账号密码来验证，点击Add，输入的账号密码并保存，记得kind选择“Username with password”

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503497955-784cd04f-f920-4cfc-9451-482079c51fe4.png)

选后Credentials选中刚刚新增的账号密码，红色提示消失；

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503498031-b866372e-6240-402e-886c-bc44a141bd9c.png)

使用ssh协议去获取代码
------------

 Repository URL填写ssh的git地址，此时未选择相应的Credentials，会有图中红色提示

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503498140-7a1dcb11-8bf7-4820-a35e-7b67788a38ae.png)

点击Add，需要添加ssh的credentials，这里需要上传的是**私钥（不是公钥！！）**

私钥文件存放在~/.ssh/id\_rsa 文件中，  
![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503498248-ece2661d-a68b-48c6-bdaa-c3f69b3b1493.png)  
可参考[http://blog.csdn.net/gw569453350game/article/details/51911179](http://blog.csdn.net/gw569453350game/article/details/51911179)

2\. 查看是否成功获取git代码
-----------------

首先可以查看jenkins的控制台输出日志，是否报错

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503498339-c45ddedb-bc07-416c-8835-bf13d40a4dce.png)

或者直接去到jenkins的工作目录

/var/lib/jenkins/workspace/

查看代码是否clone下来

  
  

**疑问点（公钥和私钥的理解）**

为什么linux上直接可以git clone成功，而用jenkins去执行不成功呢，然后jenkins为什么不用公钥而要密钥呢

个人理解：ssh生成的公钥和私钥是一对的，我们在linux上通过ssh协议获取git代码，也是先在linux生成公钥+私钥，然后把公钥上传到git服务端，然后获取代码的流程是：

a. linux把公钥上传到git服务器；

b. git服务器使用公钥加密信息（这里指代码），把信息传回给linux；

c. linux拿到信息后，通过本地的私钥解密信息，得到代码；

**而公钥私钥存放在～/.ssh下，每个用户都不一样**

而jenkins的执行是用jenkins用户去执行的，当git服务把信息给到jenkins的时候，jenkins在他的～/.ssh下没有对应的私钥，因此需要将私钥告诉jenkins，这就是jenkins为什么需要添加私钥；

