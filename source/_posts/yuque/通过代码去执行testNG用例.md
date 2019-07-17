
---

title: 通过代码去执行testNG用例

date: 2019-04-17 20:14:38 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景

用testNG去编写的测试用例，通过@Test去执行用例，一般本地都是通过IDE去执行相应的方法，持续集成的话，都是通过maven来执行或指定testNG.xml执行，但是如果想通过接口/界面去执行测试用例呢？

<a name="52b36576"></a>
# 步骤

testNG其实提供了两种通过代码执行的方法

1. 通过class来执行，show the code：

```java
TestNG testNG = new TestNG();
testNG.setTestClasses(new Class[] {WebTestFactory.class});
testNG.run();
```

新建一个TestNG对象，setTestClass传递一个用例的class进去，然后执行用例run();

2. 通过suite来执行，show the code：

```java
XmlSuite suite = new XmlSuite();
suite.setName("TmpSuite");
XmlTest test = new XmlTest(suite);
test.setName("TmpTest");
List<XmlClass> classes = new ArrayList<XmlClass>();
classes.add(new XmlClass("test.failures.Child"));
test.setXmlClasses(classes) ;
        

List<XmlSuite> suites = new ArrayList<XmlSuite>();
suites.add(suite);
TestNG tng = new TestNG();
tng.setXmlSuites(suites);
tng.run();
```

<a name="2432b575"></a>
# 备注

想直接调用指定的方法的话，需要自己折腾下，后续补上

//TODO

