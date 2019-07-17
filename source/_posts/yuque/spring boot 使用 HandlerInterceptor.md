
---

title: spring boot 使用 HandlerInterceptor

date: 2019-04-16 20:19:53 +0800

tags: []

---
<a name="8e1b944f"></a>
# 背景

在实际项目中，接口出于安全考虑，都会有验签的计算。目前接触的项目来看基本都是时间戳+干扰因子 然后md5计算的方式。现在学习，写一个简单demo，

其实如果不引入拦截器的话，验签计算全部在controller层实现也是可以的，但每个请求都需要去做一次计算，这种把公共功能的抽离，针对于所有请求前的判断，个人感觉有点切面的意思；

<a name="2a0459f9"></a>
# # DEMO

<a name="8c6ab814"></a>
## 核心点：
1. controller层还是和原来的一模一样，不做修改

2. 创建一个ApiSignInterceptor 类 ，实现HandlerInterceptor 接口，完成 验签计算的核心代码；

3. 创建一个WebConfig类，继承WebMvcConfigurationSupport类，引入步骤2中创建的拦截器；

<a name="141f3596"></a>
## 前言：
jdk8+spring boot2.0 版本 如果低版本些许不一致
<a name="3600dd9d"></a>
## show CODE

controller层：
```java
@RestController
public class PeopleController {

    @GetMapping(value = "/1/people/{people_id}")
    public String getPeopleInfo(@PathVariable(value = "people_id", required = true) String peopleId) {
        return "hello world, this is people info of " + peopleId;
    }


    @GetMapping(value = "/2/people/{people_id}")
    public String getPeopleInfoV2(@PathVariable(value = "people_id", required = true) String peopleId) {
        return "hello THIS is v2 world, this is people info V2 of " + peopleId;
    }
}
```

}

没有任何变化，简单demo例子

拦截器，ApiSignInterceptor ：

```java
public class ApiSignInterceptor implements HandlerInterceptor {


    private final static String SEPERATOR = "_";
    private final static String SECRET = "jwentest";
    private final static String NO_PERMISSION_ERROR_MESSAGE = "Api Token Error, You have no permission to access this api";


    // md5计算
    private String md5Hex(String data) {
        return DigestUtils.md5Hex(data).toLowerCase();
    }

    private String getSign(String t) {
        return md5Hex(t + SEPERATOR + SECRET);
    }

    // sign计算，t为时间戳,sign为md5(t+"_"+"jwentest")

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        try {
            String t = request.getParameter("t");
            String sign = request.getParameter("sign");

            if (t.isEmpty() || sign.isEmpty()) {
                response.sendError(403, NO_PERMISSION_ERROR_MESSAGE);
                return false;
            }

            String expectedSign = getSign(t);

            if (!expectedSign.equals(sign)) {
                response.sendError(403, NO_PERMISSION_ERROR_MESSAGE);
                return false;
            }

        } catch (Throwable t) {
            response.sendError(403, NO_PERMISSION_ERROR_MESSAGE);
            return false;
        }

        return true;

    }

}
```


其中HandlerInterceptor 接口定义了三个方法，第一次看到我有点懵逼了，为啥接口定义的方法里面会有方法体呢，为什么可以不实现所有的方法了的，原因是JDK8中可以这样写了：

```java
default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
    }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
    }
```

本次我们使用到的是preHandle方法，三个方法的执行顺序如下：

preHandler -> Controller -> postHandler -> model渲染-> afterCompletion

因此可以在进入controller层之前拦截判断是否符合我们的安全要求；

使用，WebConfig 类：


```java
@Configuration
public class WebConfig extends WebMvcConfigurationSupport {

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new ApiSignInterceptor()).addPathPatterns("/1/people/**").excludePathPatterns("/2/people/**");
        super.addInterceptors(registry);
    }
}
```


这里是在项目引入拦截器，

@Configuration，config形式加载在容器中<br />其中addPathPatterns 和 excludePathPatterns 方法，从方法名就可以看出来，是针对拦截器的范围控制，上面的代码就是针对/1/people/** 生效，对/2/people/**  不生效

目录结构如下：

![](https://cdn.nlark.com/yuque/0/2019/png/92887/1555417189381-e54a2bdc-aed9-45ff-85f2-6f969952e010.png#align=left&display=inline&height=231&originHeight=231&originWidth=261&status=done&width=261)

