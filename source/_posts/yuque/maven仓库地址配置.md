
---

title: maven仓库地址配置

date: 2019-04-17 19:55:11 +0800

tags: []

---
<a name="42550742"></a>
# # 背景

maven中央存库在国外，访问缓慢，一般国内镜像，这里推荐阿里云的 [http://maven.aliyun.com/nexus/content/groups/public](http://maven.aliyun.com/nexus/content/groups/public)

我之前采用的方式是修改setting.xml文件，后续跟研发了解下，实际使用的时候不会修改这个玩意，团队协作因为你不能强制别人去修改本地setting.xml的

而是在pom.xml中设置仓库地址就可以的

<a name="80ed3385"></a>
# # 具体

pom.xml中project节点新增如下：

```xml
<repositories>
        <!-- 远程仓库地址 -->
        <repository>
            <id>nexus-aliyun</id>  
     　　　　<name>Nexus aliyun</name>  
    　　　　 <url>http://maven.aliyun.com/nexus/content/groups/public</url>  
        </repository>

</repositories>
```


