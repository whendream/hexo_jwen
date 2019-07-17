
---

title: ERR_BLOCKED_BY_CLIENT错误

date: 2019-04-25 15:50:24 +0800

tags: []

---
<a name="Ejzna"></a>
# 背景
今天在测试需求的发现一个js报错，研发小哥说自己的电脑没有出现这个问题<br />![image.png](https://cdn.nlark.com/yuque/0/2019/png/92887/1556178719782-ecc78aa2-56c3-4942-a235-e1231b58d4db.png#align=left&display=inline&height=207&name=image.png&originHeight=398&originWidth=1437&size=45810&status=done&width=746)

<a name="wPlPq"></a>
# 排查过程

1. 研发自己电脑重现，重现失败，那边正常
1. 确认是我这里的js加载失败，先确认代码是否更新，代码为最新代码；
1. 查看具体错误，提示ERR_BLOCKED_BY_CLIENT，研发也表示没见过，那就直接百度了
1. 发现了原来是adblock的原因，我chrome是有装这个插件的，如果暂停adblock，js加载成功

<a name="S9HuP"></a>
# 后文
本来是想找下adblock默认的过滤规则，但是没有找到，但这个js文件中带有adsource的字样，推测大概是被误伤了的

<a name="e3nSJ"></a>
# 参考
[https://www.jianshu.com/p/c3cd3f820e5b](https://www.jianshu.com/p/c3cd3f820e5b)

