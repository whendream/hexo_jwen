
---

title: 通过jenkins API去build一个job

date: 2019-04-17 20:16:25 +0800

tags: []

---
背景
==

查看jenkins的api

直接访问 JENKINS\_URL/job/JOB\_NAME/api/ 就可以查看jenkins的api

build一个job的话，是POST请求 JENKINS\_URL/job/JOB\_NAME/build

会提示：

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503370152-a9bbac06-249c-447e-8358-22a6e5c69881.png)

这是jenkins的安全策略导致的，需要传递一个crumb

 解决方法
=====

有两个方案，

第一种方案：

1\. 先去掉jenkins的安全策略设置，如图，去掉勾选

在jenkins全局安全设置中 取消勾选 “防止跨站点请求伪造（Prevent Cross Site Request Forgery exploits）”

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503370237-1a3f3726-1e14-449c-ad5a-1de721ffc796.png)

2\. 允许anonymous 访问，如下图，勾选上

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503370333-0fbe242c-7a64-4cb1-aaab-3b0a3ba78dd4.png)

3\. 设置token，token是针对指定job的，所以去到job中去设置

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503370436-444176c0-8296-499c-9577-ba8b1edefa74.png)

那么就可以通过POST请求访问 JENKINS\_URL/job/test1/build?token=TOKEN\_NAME 触发这个job了

第二种方案： **安全这块不用去掉防止跨站点请求伪造，通过传递crumb来实现；但允许anonymous 访问还是要设置的**

 POST请求的时候带上这个Jenkins-Crumb（推荐这种方法）

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503370533-0efa7f64-0e3a-44ce-8da0-3a98aab780d4.png)

 访问 JENKINS\_URL/crumbIssuer/api/json 就可以获取到你的crumb，当然不同的客户端去访问得到的不一样

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503370627-13df13be-0d81-4d0e-b24d-66febecd7661.png)

备注
==

将文中的JENKINS\_URL替换成你自己的jenkins地址，

JOB\_NAME替换成job的名字；

TOKEN\_NAME 替换成你自己写的token值，如我上面的jwentest1

