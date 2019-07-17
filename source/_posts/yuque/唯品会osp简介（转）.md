
---

title: 唯品会osp简介（转）

date: 2019-04-16 20:16:13 +0800

tags: []

---
> 转自 https://blog.csdn.net/panyongcsd/article/details/58617810

> 公司(VIP)从2015年开始在内部推动Venus框架的使用，这是一款基于Apache Thrift远程调用框架二次开发的高性能、高可扩展的、服务治理的RPC框架。服务端使用IDL进行服务的定义，客户端集成服务的SDK即可调用服务端的服务，开发简单，大部分的公共功能都在Proxy代理层工作，减轻了开发者的负担，使其只需要关注业务部分。下面是对该框架的基本原理的简单介绍。   
> 参考文献:   
> 1. [Apache Thrift - 可伸缩的跨语言服务开发框架](https://www.ibm.com/developerworks/cn/java/j-lo-apachethrift/)   
> 2\. 公司内部的Venux文档(内网文档，无法分享)

一、Thrift简介
----------

Thrift采用接口描述语言定义并创建服务，支持可扩展的跨语言服务开发，使用代码生成引擎可以在多种语言之中创建高效、无缝的服务，采用二进制格式进行数据的传输，相对于xml和json体积更小，对于高并发、大数据量的环境更有优势。   
1\. Hello World示例   
Hello.thrift

     namespace java service.demo 
      service Hello{ 
           string helloString(1:string para) 
         i32 helloInt(1:i32 para) 
         bool helloBoolean(1:bool para) 
         void helloVoid() 
         string helloNull() 
      }

这段IDL定义了服务的名称和五个方法，Thrift是对IDL的一种具体实现，使用Thrift的工具编译该idl文件，就会生成相应的Hello.java文件。该文件包含了在Hello.thrift文件中描述的服务Hello的接口定义，即Hello.Iface接口，以及服务调用的底层通信细节，包括客户端的饿调用逻辑Hello.Client以及服务端的处理逻辑Hello.Processor，用于构建客户端和服务端的功能。   
创建HelloServiceImpl.java文件并实现Hello.java文件中的Hello.Iface接口，代码：

    package service.demo; 
     import org.apache.thrift.TException; 
     public class HelloServiceImpl implements Hello.Iface { 
        @Override 
        public boolean helloBoolean(boolean para) throws TException { 
            return para; 
        } 
        @Override 
        public int helloInt(int para) throws TException { 
            try { 
                Thread.sleep(20000); 
            } catch (InterruptedException e) { 
                e.printStackTrace(); 
            } 
            return para; 
        } 
        @Override 
        public String helloNull() throws TException { 
            return null; 
        } 
        @Override 
        public String helloString(String para) throws TException { 
            return para; 
        } 
        @Override 
        public void helloVoid() throws TException { 
            System.out.println("Hello World"); 
        } 
     }

1.  Thrift架构   
    架构图如下：   
    ![](https://cdn.nlark.com/yuque/0/2019/jpeg/92887/1555416965096-636cbaa3-4557-418b-9de6-627c5b1b6197.jpeg)   
    图中黄色部分是用户的业务代码，褐色部分是编译工具生成的代码框架，紫色部分和蓝色部分是我们所选择的传输层和传输协议。   
    Thrift服务器包含用于绑定协议和传输层的基础架构，它提供阻塞、非阻塞、单线程和多线程的模式运行在服务器上。
2.  传输协议   
    Thrift可以让用户选择客户端和服务端之间的传输通信协议的类别，分为文本(text)和二进制(binary)。   
    *   TBinaryProtocol：二进制编码格式进行数据传输
    *   TCompactProtocol：高效率的密集的二进制编码协议
    *   TJSONProtocol：使用Json的数据编码协议
    *   TSimpleJSONProtocol：只提供Json读写的协议，适用于脚本语言解析
3.  传输层   
    *   TSocket：使用阻塞式IO进行传输
    *   TFramedTransport：非阻塞方式，按块的大小进行传输，类似NIO；服务器必须为非阻塞的服务类型
    *   TNonblockingTransport：非阻塞模式，用于构建异步客户端
4.  服务端类型   
    *   TSimpleServer：单线程服务器，阻塞IO
    *   TThreadPoolServer：多线程服务器，阻塞式IO
    *   TNonblockingServer：多线程服务器，非阻塞IO

二、Venus的配置中心
------------

配置中心实现了集中配置、主动推送、规范配置、配置可读等优点。

三、OSP详解
-------

开放服务平台(venus-osp)是Venus体系的核心组成部分之一， 主要目标是提供服务化的核心远程调用机制以及基础服务治理功能。契约化的服务接口保证系统间的解耦清晰、干净；基于Thrift的通信和协议层确保系统的高性能；服务可以自动注册并被发现，易于部署；配合配置中心，服务配置可以动态更新；客户端与治理逻辑的分离使服务接入得到极大简化；除此之外，OSP提供了丰富的服务治理能力，如路由、负载均衡、服务保护和优雅降级等。   
1\. 远程调用机制   
![](http://i.imgur.com/0JjEErZ.png)

OSP是一套高性能、高可扩展的远程过程调用(RPC)框架，基于Apache Thrift作为基本框架，采用Thrift的 工作模式。   
每个客户端机器上都运行这一个proxy进程，该进程会将客户端的调用请求进行转发，优先转发到同机器的服务层、其次是同机房的服务、如果本机的proxy出现问题，会使用中央代理集群进行备份替换；   
客户端在请求的时候和proxy保持长连接，proxy和服务容器保持长连接，从而获得服务容器中的服务的动态信息，如机器的健康状况、配置信息、新增机器等。   
2\. 服务协议(OSP Protocol)   
OSP采用了基于二进制的通讯协议，以消息为基本通讯单元；每条消息包含消息头和消息体。协议设计上以无状态为原则，每条消息包含调用所需的所有信息，一次调用通过一次消息交换(请求/响应)完成。   
3\. 服务提供方(服务端)   
\- 服务端包含服务容器和服务本身。服务容器集中管理共享功能；服务本身以业务逻辑为主；   
\- OSP服务端容错基于无状态服务的理念，服务实例之间互不感知，通过代理层的错误感知和负载均衡等功能自动摘除有问题的服务器；   
\- 每个服务启动时，首先从配置中心获取配置，然后将自己的信息(ID、地址和端口)注册到服务注册中心；服务代理层(Proxy)从注册中心获取当前服务提供方的所有实例，通过设定的负载均衡策略对服务进行调用。   
4\. 服务代理层(OSP-Proxy)   
![](http://i.imgur.com/ihYSWxI.png)   
Proxy集中了绝大部分的服务治理智能，可以说是远程调用的大脑。   
\- Proxy部署在每台服务器上，已进程的方式运行，客户端将请求发送到Proxy进程，Proxy根据服务治理逻辑(负载均衡、路由等)对请求进行处理(转发、降级或拒绝)，这样使服务治理的实现保持对客户端和服务端的完全透明，且动态可控。   
\- 出于容错考虑，每个IDC(机房)都会部署一个Proxy集群作为备用，当本机Proxy出现故障时，自动切换到备用Proxy集群。   
5\. 服务治理   
这里的所谓服务治理主要集中在对负载均衡，路由选择以及自我保护等领域所提供的功能以及灵活性。   
![](http://i.imgur.com/OoMVUln.png)   
\- 负载均衡：OSP的负载均衡功能由osp-proxy提供，目前支持Least Active First, RR，Random等多种负载均衡策略；通过与配置中心集成，策略可以动态变更；另外，在支持基本服务端健康度检测（如ping）的基础上，利用对丰富的服务信息的统计（调用流量、延迟、错误率等），可以提供更加智能的健康检测机制。   
\- 路由选择：服务路由决定一个服务请求是否应该得到处理，以及由哪一个服务实例处理。这部分是各种服务治理政策的执行地，是一个极为重要的模块。大量的使用场景都可以结合服务路由来实现，如服务的上线/下线，在线测试，机房选择，A/B测试，灰度发布，流量控制，权限控制以及优雅降级等。   
6\. 服务管理   
\- 注册：OSP通过服务容器加载启动，服务容器需要从配置中心获取服务的配置信息，服务容器根据获取的参数，对服务进行初始化操作。服务启动成功后，容器向服务注册中心（Service Registry）进行注册，系统记录该服务的一个实例已经运行。服务容器与服务注册中心保持长连接，当服务卸载或者服务容器故障退出时，服务注册中心可以自动的删除服务实例，维护最新有效的服务实例列表。   
\- 发现：当代理层需要调用一个服务时，代理层查询服务注册中心，获取所需服务的全部服务实例，并结合服务路由策略（决定可以为当前服务请求提供服务的实例）及负载均衡策略（选择那一个服务实例进行服务），选择一个服务实例。代理层维护到服务注册中心的长连接，这样如果有新的服务实例或者老的服务实例失效时，代理层可以及时获得最新的服务实例列表。与服务端不同，当代理层失去长连接后，仍然可以通过之前获得的实例列表进行服务调用。   
\- 服务路由策略：OSP Proxy查询服务注册中心，获取所需服务的全部服务实例列表，OSP Proxy选择被调用的服务实列策略如下：   
\- 本地主机服务实例优先，OSP Proxy优先先择与其部署在同一个主机上的服务实例；   
\- 同机房服务实例优先，OSP Proxy优先先择与其部署在同机房主机上的服务实例，同机房的判断条件：主机ipv4地址前两段相同；   
\- 多个务服实例采用LeastActive(最少当前连接数，策略表示服务请求优先选择最少操作的服务器进行请求处理)选择服务实例进行。
