
---

title: glide install 问题

date: 2019-05-10 19:47:15 +0800

tags: []

---
<a name="m3IOj"></a>
# 背景 
go项目，使用glide install命令去下载安装依赖，依赖中有个github.com/hashicorp/consul

<a name="dw95Z"></a>
# 问题描述
一直无法下载安装依赖成功，报错如下：

```
[ERROR] Export failed for github.com/hashicorp/consul: Unable to export source: exit status 128
[ERROR] Unable to export dependencies to vendor directory: Unable to export source: exit status 128
```

<a name="T1bDZ"></a>
# 解决思路
> 先日了狗表达心情！

一顿google，发现提供的思路都差不多为：

```
glide cc

rm -rf vendor
```
尝试了无数次后都是失败了<br />在google的过程中，有的人建议贴出debug日志，无奈最后就自己加上了debug：glide --debug install，得到了如下日志

<a name="BvYcu"></a>
# 具体日志

```
[ERROR] Export failed for github.com/hashicorp/consul: Unable to export source: exit status 128

[DEBUG] Output was: error: unable to create file C:\\\Users\\\M\\\AppData\\\Local\\\Temp\\\glide-vendor249536483\\\vendor\\\github.com\\\hashicorp\\\consul\\\vendor/github.com/hashicorp/go-discover/provider/azure/vendor/github.com/Azure/azure-sdk-for-go/services/network/mgmt/2015-06-15/network/expressroutecircuitauthorizations.go: Filename too long[DEBUG]

Unlocking https-github.com-hashicorp-consul

[ERROR] Unable to export dependencies to vendor directory: Unable to export source: exit status 128

[DEBUG] Output was: error: unable to create file C:\\\Users\\\M\\\AppData\\\Local\\\Temp\\\glide-vendor249536483\\\vendor\\\github.com\\\hashicorp\\\consul\\\vendor/github.com/hashicorp/go-discover/provider/azure/vendor/github.com/Azure/azure-sdk-for-go/services/network/mgmt/2015-06-15/network/expressroutecircuitauthorizations.go: Filename too long
```
日了狗，看描述大概明白了，glide在执行新建文件的时候发现文件名过长了！！！

<a name="KN3wD"></a>
# 解决办法
那么问题就演变成了glide error filename too long的问题了<br />具体方法：

```
Enable long paths on Windows (requires Windows 10 Anniversary Update or newer): https://superuser.com/a/1119980/97078

Configure git to use long paths: git config --global core.longpaths true (globally) or git config core.longpaths true (per project)
```
我执行了第二步就解决了问题，那么大概就是glide 会去使用git去拉取代码和创建文件了的 

<a name="8KWU0"></a>
# 小结
还是问题的官方日志靠谱，遇到问题先获取更多的日志吧

