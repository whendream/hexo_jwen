
---

title: jacoco初探

date: 2019-04-16 20:02:02 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景
集团的代码覆盖率平台因为网络问题无法使用，只能自己研究下。

覆盖率是衡量自动化用例效果产品的一个指标，但只是一个辅助指标，覆盖率高并不意味着质量好，但覆盖率低却能说明一些问题，

<a name="f5deaa17"></a>
# 对比

覆盖率工具的对比，直接引用资料：


![image.png](https://cdn.nlark.com/yuque/0/2019/png/92887/1555416228795-02bd1341-65f0-4914-a10b-b6949ad30136.png#align=left&display=inline&height=517&name=image.png&originHeight=517&originWidth=738&size=133429&status=done&width=738)

> 有赞团队的博客： [https://tech.youzan.com/code-coverage/](https://tech.youzan.com/code-coverage/)


<a name="ac8a4f56"></a>
# 理解

1. 结合业务形态，被测服务不能停止服务；

2. 通过javaagent方式去启动jacoco；

3. javaagent的方式可以用file，tcpserver、tcpclient三种模式，常用的是tcpserver格式

4. 挂载javagent后，可以利用ip：port来跟javaagent进行网络交互，生成exec文件，生成报告；

<a name="74718689"></a>
# 细节

1. jacoco官网：[https://www.eclemma.org/jacoco/](https://www.eclemma.org/jacoco/) 上去下载agent.jar包；

2. javaagent格式：

-javaagent:_[yourpath/]_jacocoagent.jar=_[option1]_=_[value1]_,_[option2]_=_[value2]<br />
更多参数：[https://www.jacoco.org/jacoco/trunk/doc/agent.html](https://www.jacoco.org/jacoco/trunk/doc/agent.html)<br />
_

_ 实际例子：-javaagent:/home/tools/jacocoagent.jar=includes=*,output=tcpserver,address=xxx.xxx.xx.xx,port=6300,append=true_

3. 生成exec文件不局限于ant工具，其实底层还是通过tcp连接去访问；

4. 生成exce后需要解析成报告，比较麻烦，要有编译后的class文件也有要源码。最理解的状态应该是从服务器拿回本地来操作；

5. 实际落地： 被测服务挂载javaagent --》执行自动化用例 --》 生成exec文件 --》 解析生成报告（被测服务器上生成exec文件）

<a name="9822c60d"></a>
# 疑问

1. jacoco只支持时间段的代码覆盖率的统计，并不能细化到哪个方法/接口；

2. javaagent的tcpserver和tcpclient的区别是什么？翻阅文档基本上都是tcpserver的，没有用tcpclient的

