# 一：XML

1. 概念：可扩展标记语言

   可扩展：标签都是自定义的.<user>

2. 功能

   存储数据

   * 配置文件
   * 在网络中传输

3. xml与html的区别

   * xml标签都是自定义的，html标签是预定义
   * xml的语法严格，html语法松散
   * xml是存储数据的，html是展示数据

4. 语法
   * xml文档名的后缀名  .xml
   * xml第一行必须定义为文档声明
   * xml文档中有且只有一个根标签
   * 属性值必须使用引号引起来
   * 标签必须正确关闭
   * xml标签名区分大小写

5. 解析:操作xml文档，将文档中的数据读取到内存中

   (1)操作xml文档

   * 解析(读取)：将文档中的数据读取到内存中
   * 写入：将内存中的数据保存到xml文档中，持久化的存储

   (2)解析xml的方式：

   * DOM：将标记语言文档一次性加载进内存，在内存中形成一颗dom树

     ​	优点：操作方便，可以对文档进行CRUD操作

     ​    缺点：占内存

   * SAX：逐行读取，基于事件驱动

     ​	 优点：不占内存

     ​	 缺点：只能读取，不能增删改

# 二：servlet

## (1)、执行流程

   1.浏览器向服务器发出GET请求(请求服务器ServletA)

​	2.服务器上的容器逻辑接收到该url,根据该url判断为Servlet请求，此时容器逻辑将产	生两个对象：请求对象(HttpServletRequest)和响应对象(HttpServletResponce)

​	3.容器逻辑根据url找到目标Servlet(本示例目标Servlet为ServletA),且创建一个线程A

​	4.容器逻辑将刚才创建的请求对象和响应对象传递给线程A

​	5.容器逻辑调用Servlet的service()方法

​	6.service()方法根据请求类型(本示例为GET请求)调用doGet()(本示例调用doGet())或	doPost()方法

​	7.doGet()执行完后，将结果返回给容器逻辑

​	8.线程A被销毁或被放在线程池中

​	注意：

​	1.在容器中的每个Servlet原则上只有一个实例

​	2.每个请求对应一个线程

​	3.多个线程可作用于同一个Servlet(这是造成Servlet线程不安全的根本原因)

​	4.每个线程一旦执行完任务，就被销毁或放在线程池中等待回收

## (2) 、servlet接口的五个方法



## (3)、http

1. 传输协议：定义了客户端和服务器端通信时发送数据的格式

2. 特点：

   * 基于TCP/IP的高级协议
   * 默认端口号：80
   * 基于请求/响应模型的：一次请求对应一次响应
   * 无状态的：每次请求之间相互独立，不能交互数据

3. 历史版本

   * 1.0：每一次请求响应都会建立新的连接
   * 1.1：服用连接

4. 请求头（常见的请求头）

   * User-Agent:浏览器告诉服务器，我访问你使用的浏览器版本信息

   * Referer：http://localhost/login.html

     （1）告诉服务器我从哪里来

     （2）作用

     ​		a.防盗链

     ​		b.统计工作

# 三、会话

## 1.概念

* 会话：一次会话中包含多次请求和响应。

  1. 一次会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止

* 功能：在一次会话的范围内的多次请求间，共享数据

* 方式：

  1. 客户端会话技术：cookie
  2. 服务端会话技术：session

* 什么是cookie

  1. HTTP协议本身是无状态的。什么是无状态呢，即服务器无法判断用户身份。Cookie实际上是一小段的文本信息（key-value格式）。客户端向服务器发起请求，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。

* cookie机制

  1. 当用户第一次访问并登陆一个网站的时候，cookie的设置以及发送会经历以下4个步骤：

     客户端发送一个请求到服务器 --》 **服务器发送一个HttpResponse响应到客户端，其中包含Set-Cookie的头部** --》 **客户端保存cookie，之后向服务器发送请求时，HttpRequest请求中会包含一个Cookie的头部** --》**服务器返回响应数据**

## 2.cookie

* 概念：客户端会话技术，将数据保存到客户端
* 使用步骤
  1. 创建cookie对象，绑定数据
  2. 发送cookie对象
  3. 获取cookie，拿到数据
* 实现原理
  1. 基于响应头set-cookie和请求头cookie实现
* cookie共享
  1. 假设在一个tomcat服务器中，部署了多个web项目，那么在这些web项目中cookie能不能共享？
     * 默认情况下不能共享
     * setPath(String path)：设置cookie的获取范围。默认情况下，设置当前的虚拟目录，将path设置为"/"
  2. 不同的tomcat服务器间cookie共享问题？
     * setDomain(String path):如果设置一级域名相同，那么多个服务器之间cookie可以共享，例如：setDomain(".baidu.com")，那么tieba.baidu.com和news.baidu.com中cookie可以共享

## 3.session

* 概念：服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中

* 原理：

  1. session的实现是依赖于cookie的

* 细节：

  1. 当客户端关闭后，服务器不关闭，两次获取session是否为同一个？

     * 默认情况下不同
     * 如果需要相同，则可以创建cookie，键为JSESSIONID，设置最大存活时间，让cookie持久化保存

  2. 客户端不关闭，服务器关闭后，两次获取的session是同一个吗？

     * 不是同一个，但是要确保数据不丢失

       1. session的钝化：

          在服务器正常关闭之前，将session对象序列化到硬盘上

       2. session的活化：

          在服务器启动后，将session文件转化为内存中的session对象即可

  3. session什么时候被销毁

     * 服务器被关闭
     * session调用invalidate()
     * session默认失效时间30分钟

  4. session的特点

     * session用于存储一次会话的多次请求的数据，存在服务器端
     * session可以存储任意类型，任意大小的数据

## 4.session和cookie的区别

* session存储数据在服务端，cookie在客户端，
* session没有数据大小限制，cookie有
* session数据安全，cookie相对不安全

# 四、JSP

## 1、九大内置对象

* pageContext：当前页面共享数据
* request：一次请求访问的多个资源（转发）
* session：一次会话的多个请求间
* application：所有用户间共享数据
* response：响应对象
* page：当前页面
* out：输出对象，数据输出到页面上
* config：Servlet的配置对象
* exception：异常对象