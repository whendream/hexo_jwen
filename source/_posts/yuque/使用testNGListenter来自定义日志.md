
---

title: 使用testNGListenter来自定义日志

date: 2019-04-17 20:12:00 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景

用testNG写用例的时候，只是打印了请求的日志，没有打印这个用例的开始和结束的标识，想加上这个标识这样更好的排查问题

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503118908-2f222be0-6c2b-4d2c-86b2-a45fec5550e3.png#align=left&display=inline&height=260&originHeight=310&originWidth=889&status=done&width=746)

这种日志是加在用例开始执行和结束，相当于spring中的AOP功能，今天翻阅了testNG的文档发现有监听器这玩意，这玩意可以在testNG执行的某一过程中进行操作；

<a name="1c53b310"></a>
# 操作步骤

直接show the code：

```java
public class TestNGLogListener extends TestListenerAdapter {

    static Logger logger = LogManager.getLogger(TestNGLogListener.class.getName());

    @Override
    public void onTestFailure(ITestResult tr) {
        log(String.format("[method: %s]",tr.getName())+ "--Test method failed\n");
    }

    @Override
    public void onTestSkipped(ITestResult tr) {
        log(String.format("[method: %s]",tr.getName())+ "--Test method skipped\n");
    }

    @Override
    public void onTestSuccess(ITestResult tr) {
        log(String.format("[method: %s]",tr.getName())+ "--Test method success\n");
    }

    @Override
    public void onTestStart(ITestResult tr) {
        log(String.format("[method: %s]",tr.getName())+ "-- START");
    }

    private void log(String string) {
        logger.info(string);
    }
}
```


1. 写编写一个监听器的类，继承 TestListenerAdapter 这个类，需要重写对应的几个方法

```java
void onTestFailure(ITestResult result) // 用例执行结果失败
void onTestSkipped(ITestResult result) // 跳过该条用例
void onTestSuccess(ITestResult result) // 用例执行结果成功
void onTestStart(ITestResult tr) // 用例开始执行的时候
```

2. 使用这个监听器，有两种方法

2.1 第一种是直接在测试用例的class上加Listener注解，如下：

```java
@Listeners({CustomListener.class })
public class SampleTest {

    @Test
    public void testMethodOne(){
        Assert.assertTrue(true);
    }

    @Test
    public void testMethodTwo(){
        Assert.assertTrue(false);
    }

    @Test(dependsOnMethods={"testMethodTwo"})
    public void testMethodThree(){
        Assert.assertTrue(true);
    }

}
```


2.2 直接在testNG.xml文件中添加，如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="wm-api-autotest">
    <test name="Test">
        <packages>
            <package name="com.jwen.demo"/>
        </packages>
    </test>
    <listeners>
        <listener class-name=com.jwen.demo.common.TestNGLogListener'/>
    </listeners>
</suite> <!-- Suite -->
```

2.3 效果展示：

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555503119013-41ca3e7b-220e-4b0b-bd67-5367bd7441ad.png#align=left&display=inline&height=88&originHeight=140&originWidth=1191&status=done&width=746)


