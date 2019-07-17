
---

title: 关于nginx

date: 2019-07-16 19:57:13 +0800

tags: []

---
<a name="LVWhw"></a>
# What
Nginx是一款轻量级的Web服务器、反向代理服务器，由于它的内存占用少，启动极快，高并发能力强，在互联网项目中广泛应用。

![](https://cdn.nlark.com/yuque/0/2019/jpeg/92887/1563278513924-0ae579e4-e8a4-4bff-be76-d58bb7778cba.jpeg#align=left&display=inline&height=506&originHeight=506&originWidth=688&size=0&status=done&width=688)

<a name="rrTrs"></a>
## 反向代理
![image.png](https://cdn.nlark.com/yuque/0/2019/png/92887/1563278539375-dfe54c83-19fb-4c78-8aa9-56e6086f4a8d.png#align=left&display=inline&height=401&name=image.png&originHeight=501&originWidth=441&size=147840&status=done&width=352.8)<br />当我们在外网访问百度的时候，其实会进行一个转发，代理到内网去，这就是所谓的反向代理，即反向代理“代理”的是服务器端，而且这一个过程对于客户端而言是透明的。

服务器根据客户端的请求，从其关联的一组或多组后端服务器（如Web服务器）上获取资源，然后再将这些资源返回给客户端，客户端只会得知反向代理的IP地址，而不知道在代理服务器后面的服务器簇的存在

<a name="ryksr"></a>
## 正向代理
![](https://cdn.nlark.com/yuque/0/2019/jpeg/92887/1563278625606-3f0b6177-812c-4608-93c7-9f213a7ab3c8.jpeg#align=left&display=inline&height=269&originHeight=269&originWidth=349&size=0&status=done&width=349)<br />由于防火墙的原因，我们并不能直接访问谷歌，那么我们可以借助VPN来实现，这就是一个简单的正向代理的例子。这里你能够发现，正向代理“代理”的是客户端，而且客户端是知道目标的，而目标是不知道客户端是通过VPN访问的。

<a name="CpWyV"></a>
# 工作方式
Master-Worker模式<br />![](https://cdn.nlark.com/yuque/0/2019/jpeg/92887/1563278687914-b9c2c1b0-1df3-43a9-83cf-d8a867773389.jpeg#align=left&display=inline&height=446&originHeight=446&originWidth=599&size=0&status=done&width=599)<br />启动Nginx后，其实就是在80端口启动了Socket服务进行监听，如图所示，Nginx涉及Master进程和Worker进程。

<a name="Rzcsb"></a>
# 安装
这个参考的是中文官网，但注意的是，官网上依赖的包不是最新的，可能会找不到相应的资源，自己注意替换为最新的<br />[http://www.nginx.cn/install](http://www.nginx.cn/install)

<a name="psm0s"></a>
# 配置
我按照默认来，配置是在nginx.conf文件中

1. 修改user 参数，nginx默认启动的是nobody用户，需要修改为指定用户，不然会导致读取目录权限出错的问题；
1. server -- location 中修改root 指定到你需要指定的内容目录
1. 实际工作中，会根据host来管理conf，会把所有的conf放在vhost目录中，方便管理，只需要在nginx.conf加载vhost目录即可，如下：

![image.png](https://cdn.nlark.com/yuque/0/2019/png/92887/1563279097613-62a39124-98fa-4413-930d-0db477fa0ea6.png#align=left&display=inline&height=34&name=image.png&originHeight=43&originWidth=222&size=3636&status=done&width=177.6)

<a name="7J8hk"></a>
# 命令

```php
{nginx_home}/nginx                      # 启动nginx
{nginx_home}/nginx -s reload            # 重新载入配置文件
{nginx_home}/nginx -s reopen            # 重启 Nginx
{nginx_home}/nginx -s stop              # 停止 Nginx
```



