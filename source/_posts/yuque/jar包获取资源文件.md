
---

title: jar包获取资源文件

date: 2019-04-16 19:46:03 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景
写的一个spring boot项目打成jar包部署运行下，打成jar包，提示找不到资源文件，如下图：

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555415069829-61e77f52-aaea-4403-93fd-060b5b1ccf1c.png#align=left&display=inline&height=103&originHeight=208&originWidth=1506&status=done&width=746)

直接通过idea是可以运行的，但打成jar包后提示找不到资源文件，简单查阅后了解到是因为jar包在读取文件的方式不一致导致的

<a name="094c47ac"></a>
# 问题分析
先定位到哪行代码出错，如下：

URI configurationFileURI = this.getClass().getClassLoader().getResource(CONFIGURATION_FILE).toURI();

这里报错，提示getResource为null。

原因如下：

**在jar文件中查找资源和在文件系统中查找资源的方式是不一样的**

错误的加载方式：

XXX.calss.getResource(path)<br />
XXX.calss.getClassLoader().getResource(path)

正确的加载方式：

XXX.class.getResourceAsStream(path)<br />
XXX.calss.getClassLoader().getResourceAsStream(path)

以流的方式来加载

<a name="957a228f"></a>
# 解决方法

知道了根本原因了，就简单了，将之前getResource这种方式改成getResourceAsStream方法

具体代码如下：

```java
InputStream resourceAsStream = this.getClass().getClassLoader().getResourceAsStream(CONFIGURATION_FILE);
BufferedReader br = new BufferedReader(new InputStreamReader(resourceAsStream));

String s = "";
List lines = new ArrayList();
while ((s = br.readLine()) != null) {
lines.add(s);
}
// 关闭流
resourceAsStream.close();
br.close();
```

<a name="7187e4d5"></a>
# 简单总结

1. 在jar文件中查找资源和在文件系统中查找资源的方式是不一样的
2. jar包是一个单独的文件而非文件夹，绝对不可能通过"file:/e:/.../ResourceJar.jar/resource/res.txt"这种形式的文件URL来定位资源文件
3. public InputStream getResourceAsStream(String name); 返回读取指定资源的输入流。这个方法很重要，可以直接获得jar包中文件的内容。

<a name="35808e79"></a>
# 参考资料
[http://hxraid.iteye.com/blog/483115](http://hxraid.iteye.com/blog/483115)

