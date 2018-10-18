Java通讯模型（BIO、NIO、AIO综合演练）

1、常规技术：Spring系统、ORM组件、服务支持；
    <br>数据表的CRUD处理（重复且大量的编写），这种开发好像不是开发的感觉。</br>
    
2、未来的开发人才到底该具备哪些技能 —— 架构师
    <br>A、可以完成项目，同时可以很好的沟通；</br>
    <br>B、掌握各种常规的开发技术，并且掌握一些服务组件的使用（需要有好的运维）；</br>
    <br>C、良好的代码设计能力 —— 代码重用与标准设定；</br>
    <br>D、非常清楚底层通讯机制，并且可以根据实际的业务需求，进行底层通讯协议的定义；</br>

3、网络通讯的核心思想：请求-回应
    <br>网络七层模型：</br>
        应用层、表示层、会话层、传输层（数据段）、网络层（数据包）、数据链路层（数据帧）、物理层（比特流）。
   
    <br>TCP协议是整个现代项目开发中的基础，包括HTTP协议也都是在TCP基础上实现的。</br>
    
    
本次的案例：采用一个标准的ECHO程序
    客户端输入一个内容，随后服务器端接收到之后进行数据的返回，在数据前面追加有"【ECHO】"的信息。
    "telnet 主机名称 端口号"，主要是进行TCP协议的通讯，而对于服务器端是如何实现的并不关注。
    


<p> 
一、【JDK 1.0】实现BIO程序开发：同步阻塞IO操作，每一个线程都只会管理一个客户端的链接，这种操作的本质是存在有程序阻塞的问题。
    线程的资源总是在不断的进行创建，并且所有的数据接收里面（Scanner、PrintStream简化了），网络的数据都是长度限制的，传统的数据是需要通过字节数组的形式搬砖完成的。
    BIO：是需要对数据采用蚂蚁搬家的模式完成的。
    程序问题：性能不高、多线程的利用率不高、如果大规模的用户访问，有可能会造成服务器端资源耗尽。
</p>
<p> 
二、【JDK 1.4】提供有了一个java.nio的开发包，从这一时代开始，Java提升了IO的处理效率，同时也提升网络模型的处理效率，同时NIO里面采用的是同步非阻塞IO操作。
NIO的出现在当时来讲，给Java带来了一个最伟大的通讯利器（已经接近于底层的传输性能）。
    NIO里面提倡使用Buffer来代替传统的数组操作（蚂蚁搬家），可以减少数组的操作复杂度，利用缓存数据的保存与方便的清空操作进行处理。
    Reactor模型提倡的是：公共注册，统一操作，所以会提供有一系列的Channel（通道）。
</p>

<p>
三、【JDK 1.7】AIO，异步非阻塞IO通讯，需要采用大量的回调处理模式，所以需要使用：
            public interface CompletionHandler<V,A>
</p>

总结：
    BIO（同步阻塞IO）：在进行处理的时候是通过一个线程进行操作，并且IO实现通讯的时候采用的是阻塞模式；
           你现在通过水壶烧水，在BIO的世界里面，烧水这一过程你需要从头一直监视到结尾；
    NIO（同步非阻塞IO）：不断的进行烧水状态的判断，同时你可以做其他的事情；
    AIO（异步非阻塞IO）：烧水的过程你不用关注，如果水一旦烧好了，就会给你一个反馈。
   