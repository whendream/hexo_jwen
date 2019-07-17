
---

title: CGI FastCGI PHP-FPM

date: 2019-04-24 16:56:49 +0800

tags: []

---
<a name="CGI"></a>
# CGI
> Common Gateway Interface, CGI 是Web 服务器运行时外部程序的规范,按CGI 编写的程序可以扩展服务器功能。CGI 应用程序能与浏览器进行交互,还可通过数据库API 与数据库服务器等外部数据源进行通信,从数据库服务器中获取数据。格式化为HTML文档后，发送给浏览器，也可以将从浏览器获得的数据放到数据库中。几乎所有服务器都支持CGI,可用任何语言编写CGI

**历史在前进，静态资源 变成 动态资源的历史进程；**<br />如访问jpg这种静态资源，web server在相应的目录找到了文件返回，<br />当web server收到/index.php这个请求后，会启动对应的CGI程序，这里就是PHP的解析器。接下来PHP解析器会解析php.ini文件，初始化执行环境，然后处理请求，再以规定CGI规定的格式返回处理后的结果，退出进程。web server再把结果返回给浏览器。

缺点：<br />每次请求过来都会启动cgi程序，然后处理，然后退出，性能不好

<a name="FastCGI"></a>
# FastCGI
> FastCGI全称 快速通用网关接口（FastCommonGatewayInterface）。
> FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次(这是CGI最为人诟病的fork-and-execute 模式)。它还支持分布式的运算, 即 FastCGI 程序可以在网站服务器以外的主机上执行并且接受来自其它网站服务器来的请求。
> FastCGI是语言无关的、可伸缩架构的CGI开放扩展，其主要行为是将CGI解释器进程保持在内存中并因此获得较高的性能。

**一种技术的诞生，往往是解决一些问题。**<br />**
<a name="PHP-FPM"></a>
# PHP-FPM
> PHP-FPM(FastCGI Process Manager：FastCGI进程管理器)是一个PHPFastCGI管理器，对于PHP 5.3.3之前的php来说，是一个补丁包[ ]() ，旨在将FastCGI进程管理整合进PHP包中。如果你使用的是PHP5.3.3之前的PHP的话，就必须将它patch到你的PHP源代码中，在编译安装PHP后才可以使用。

大家都知道，PHP的解释器是php-cgi。php-cgi只是个CGI程序，他自己本身只能解析请求，返回结果，不会进程管理，所以就出现了一些能够调度php-cgi进程的程序<br />php-fpm实现了fastcgi这个协议


<a name="35808e79"></a>
# 参考资料
[https://blog.csdn.net/u010785091/article/details/78705690](https://blog.csdn.net/u010785091/article/details/78705690)<br />[https://blog.csdn.net/App_IOS/article/details/80640987](https://blog.csdn.net/App_IOS/article/details/80640987)<br />[https://segmentfault.com/a/1190000007322358?utm_source=tag-newest](https://segmentfault.com/a/1190000007322358?utm_source=tag-newest)

