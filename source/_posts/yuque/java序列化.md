
---

title: java序列化

date: 2019-04-17 19:54:23 +0800

tags: []

---
<a name="42550742"></a>
# # 背景

java对象是在jvm中，如果jvm销毁，那么对象都不存在了。如果想继续使用java对象的话，需要用到序列化，将java中的对象转化为字节序列，用于存储和运输；

那么可以将DB理解为一种序列化，将java对象序列化后存储在DB中，将java对象保存在文本中也是一种序列化

<a name="10ea5bac"></a>
# # 细节

**需要被序列化的类，需要实现Serializable接口**

虽然Serializable接口是空的，没有任何方法，但也要实现，起到标识的作用

**同一字节流中的引用是得到保存的**

User user =  new User("jwen");

Order o1 =  new Order(user, "o1");

Order o2 =  new Order(user, "o2");

将这两个对象o1和o2序列化保存在同一文本中（同一输出流），会发现两个对象的o1.user == o2.user

但将两个对象分别保存在不同文件中，file1和file2的话，引用是不一样的 o1.user != o2.user

**反序列化没有调用构造函数**

```java
public class User implements Serializable {

    private String name;
    private int id = 1;

    public User(String name, int id) {
        System.out.println("构造函数调用");
        this.name = name;

    }
}
```


这里的构造方法不会被调用

**序列化Id标识版本信息**

private static final long serialVersionUID = -5809782578272943999L;

<a name="ec030200"></a>
# # 后记

推荐博客：[https://www.cnblogs.com/xdp-gacl/p/3777987.html](https://www.cnblogs.com/xdp-gacl/p/3777987.html)

