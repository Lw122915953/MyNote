# 一、zookeeper安装与启动

​        首先需要安装JdK，从[Oracle](http://lib.csdn.net/base/oracle)的[Java](http://lib.csdn.net/base/java)网站下载，安装很简单，就不再详述。其中zookeeper的下载地址是http://www.apache.org/dyn/closer.cgi/zookeeper/，下载后直接解压，不用安装。

​        在你执行启动脚本之前，还有几个基本的配置项需要配置一下，Zookeeper 的配置文件在 conf 目录下，这个目录下有 zoo_sample.cfg 和 log4j.properties，你需要做的就是将 zoo_sample.cfg 改名为 zoo.cfg，因为 Zookeeper 在启动时会找这个文件作为默认配置文件。下面详细介绍一下，这个配置文件中各个配置项的意义。



**[html]**[view plain](http://blog.csdn.net/evankaka/article/details/47858707#)





1. \# The number of milliseconds of each tick  
2. tickTime=2000  
3. \# The number of ticks that the initial   
4. \# synchronization phase can take  
5. initLimit=10  
6. \# The number of ticks that can pass between   
7. \# sending a request and getting an acknowledgement  
8. syncLimit=5  
9. \# the directory where the snapshot is stored.  
10. \# do not use /tmp for storage, /tmp here is just   
11. \# example sakes.  
12. dataDir=D:\\Java\\Tool\\zookeeper-3.4.6\\tmp  
13. \# the port at which the clients will connect  
14. clientPort=2181  
15. \# the maximum number of client connections.  
16. \# increase this if you need to handle more clients  
17. \#maxClientCnxns=60  
18. \#  
19. \# Be sure to read the maintenance section of the   
20. \# administrator guide before turning on autopurge.  
21. \#  
22. \# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance  
23. \#  
24. \# The number of snapshots to retain in dataDir  
25. \#autopurge.snapRetainCount=3  
26. \# Purge task interval in hours  
27. \# Set to "0" to disable auto purge feature  
28. \#autopurge.purgeInterval=1  



把

**[plain]**[view plain](http://blog.csdn.net/evankaka/article/details/47858707#)





1. dataDir=/tmp/zookeeper  

改成如下：



**[html]**[view plain](http://blog.csdn.net/evankaka/article/details/47858707#)





1. **<****pre** name="code" class="html"**>**dataDir=D:\\Java\\Tool\\zookeeper-3.4.6\\tmp**</****pre****>**  

这里可以改成自己喜欢的

**[html]**[view plain](http://blog.csdn.net/evankaka/article/details/47858707#)





1. ​    tickTime：这个时间是作为 Zookeeper 服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个 tickTime 时间就会发送一个心跳。  
2. ​    dataDir：顾名思义就是 Zookeeper 保存数据的目录，默认情况下，Zookeeper 将写数据的日志文件也保存在这个目录里。  
3. ​    dataLogDir：顾名思义就是 Zookeeper 保存日志文件的目录  
4. ​    clientPort：这个端口就是客户端连接 Zookeeper 服务器的端口，Zookeeper 会监听这个端口，接受客户端的访问请求。  
5.   
6. 当这些配置项配置好后，你现在就可以启动 Zookeeper 了，启动后要检查 Zookeeper 是否已经在服务，可以通过 netstat – ano 命令查看是否有你配置的 clientPort 端口号在监听服务  



单机安装非常简单，只要获取到 Zookeeper 的压缩包并解压到某个目录如：D:\Java\Tool\zookeeper-3.4.6下，Zookeeper 的启动脚本在 bin 目录下，Windows 下的启动脚本是bin文件下的如，D:\Java\Tool\zookeeper-3.4.6\bin\ zkServer.cmd。

启动如下：

![img](https://img-blog.csdn.net/20150827140429526)

输入JPS，出现如下，也说明启动成功

![img](https://img-blog.csdn.net/20150823110117879)



上面的黑色框框不关，就表示注册中心一直开关的，一定要记得注册中心要在程序运行之前就打开，而且是一直开着的

# 二、Dubbo-admin管理平台的安装

## 1.1、dubbo-admin 本地编译打包

​        因为zookeeper只是一个黑框，我们无法看到是否存在了什么提供者或消费者，这时就要借助Dubbo-Admin管理平台来实时的查看，也可以通过这个平台来管理提者和消费者。
​        **dubbo-admin.war可在网上百度去下载，但是我下载了好几个war包，发布上去服务启动都报错，这个时候大概是我们系统的JDK和编译dubbo-admin.war的JDK版本不同导致的了。所以我之后直接下载了dubbo-master的源代码，然后自己编译了一个war包，这样就不会存在启动报错的问题了。（这里强烈建议自己编译一个，网上找的基本都不行，试了很多个，最后还是自己来搞）其中自己电脑编译的过程如下，一定在安装maven和jdk!**

dubbo的所有源码可在https://github.com/alibaba/dubbo上下载。下好之后解压

![img](https://img-blog.csdn.net/20150827140751652)

解压后的文件内容，这里你其它的都不用去管，只管dubbo-admin.

![img](https://img-blog.csdn.net/20150827141527417)

**因为这里只需要编译Dubbo-Admin，所以打开Cmd,然后进入解压路径 E:\下载\dubbo-master\dubbo-master\dubbo-admin,有人把整个工程都编译了，其实没有必要，只需要编译dubbo-admin即可，有什么依赖的，maven会自动帮你解决。整个过程如下：**

**首先，通过cmd进入目录，输入命令**

![img](https://img-blog.csdn.net/20150827141024607)

然后输入命令



**[html]**[view plain](http://blog.csdn.net/evankaka/article/details/47858707#)





1. mvn package -Dmaven.skip.test=true  

这里要自己把maven的环境变量配置好，这里的-Dmaven.skip.test表示不打包测试包。然后结果如下，表示打包成功



![img](https://img-blog.csdn.net/20150827140932257)

再打开项目的文件所在位置：E:\下载\dubbo-master\dubbo-master\dubbo-admin\target

target表示构建的本地路径，打开，里面有个文件dubbo-admin-2.5.4-SNAPSHOT.war表示打包成功，这个文件得放在Tomcat下才能运行。

![img](https://img-blog.csdn.net/20150827141345840)

这里我已上传这个war包，**下载地址：****http://download.csdn.net/detail/evankaka/9054273**

## 1.2、dubbo-admin安装



​       dubbo-admin已打包成功，接下来就容易很多了

\1. 安装
**将 dubbo-admin-2.5.4-SNAPSHOT.war 拷入 tomcat  webapps中**
\2. 配置
**修改tomcat的端口8088，修改方法如下，打到conf下的文件 server.xml，因为zookeeper会用到8080的端口，所以为了不冲突，把Tomcat的端口改一下。记得一定要改！！！**



![img](https://img-blog.csdn.net/20150827141749247)

然后修改



![img](https://img-blog.csdn.net/20150827141715250)

3、启动一下Tomcat,让它把war解压了

过程如下

![img](https://img-blog.csdn.net/20150823111252769)

然后启动

![img](https://img-blog.csdn.net/20150823111305130)

发现webapp下多了一个文件夹

![img](https://img-blog.csdn.net/20150827141952338)

再把 Tomcat给关了，直接stop,上面的按钮

4、修改dubbo.properties

D:\Java\Tool\apache-tomcat-7.0.62\webapps\dubbo-admin-2.5.4-SNAPSHOT\WEB-INF

![img](https://img-blog.csdn.net/20150827142213413)

是否是如下内容：



**[html]**[view plain](http://blog.csdn.net/evankaka/article/details/47858707#)





1. dubbo.registry.address=zookeeper://127.0.0.1:2181  
2. dubbo.admin.root.password=root  
3. dubbo.admin.guest.password=guest  

**如果是，就不用改，这里的127.0.0.1对应的是自己的电脑IP，因为这里zookeeper也在自己电脑上，所以要这么写才行。一般情况下都是不需要改的，因为初始都是本地的IP地址。但还是看看比较安全**

![1569569889995](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1569569889995.png)启动服务


\5. 访问:

**这下全部都配置好了，首先，一定要先启动zookeeper启动后再去启动tomcat！一定要先启动zookeeper启动后再去启动tomcat！一定要先启动zookeeper启动后再去启动tomcat！重要的事情三遍！**

启动zookeeper

启动tomcat

**访问http://localhost:8088/dubbo-admin-2.5.4-SNAPSHOT/ 一定要注意名一定要和你webapp下的工程名一样！！**

弹出如下：

![img](https://img-blog.csdn.net/20150827142520531)



![img](https://img-blog.csdn.net/20150827142432300)

**输入用户名和密码都是root**

就会看到：

![img](https://img-blog.csdn.net/20150827142639327)

**出现以下界面说明安装配置成功，登录密码为root/root**



这下子就大功告成了，可以开始来使用了~~~~~~~~



转载于：http://blog.csdn.net/tjeagle/article/details/50021151

注：

实际操作中，dubbo-admin部署到tomcat报错了





解决方法：



原来是我用的jdk版本太高，jdk8, dubbo默认用的[spring](http://lib.csdn.net/base/javaee)版本比较旧，冲突导致，作如下修改即可



```
ERROR context.ContextLoader - Context initialization failedorg.springframework.beans.factory.BeanCreationException: Error creating bean with name 'uriBrokerService': Cannot create inner bean '(inner bean)' of type [com.alibaba.citrus.service.uribroker.impl.URIBrokerServiceImpl$URIBrokerInfo] while setting bean property 'brokers' with key [0]; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name '(inner bean)#25': Cannot create inner bean 'server' of type [com.alibaba.citrus.service.uribroker.uri.GenericURIBroker] while setting constructor argument; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'server': Error setting property values; nested exception is org.springframework.beans.NotWritablePropertyException: Invalid property 'URIType' of bean class [com.alibaba.citrus.service.uribroker.uri.GenericURIBroker]: Bean property 'URIType'is not writable or has an invalid setter method. Does the parameter type of the setter match the return type of the getter?        at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveInnerBean(BeanDefinitionValueResolver.java:230)        at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:122)        at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveManagedList(BeanDefinitionValueResolver.java:287)
```







1、webx的依赖改为3.1.6版；

```
    <dependency>        <groupId>com.alibaba.citrus</groupId>        <artifactId>citrus-webx-all</artifactId>        <version>3.1.6</version>    </dependency>
```

2、添加velocity的依赖，我用了1.7；

```
    <dependency>        <groupId>org.apache.velocity</groupId>        <artifactId>velocity</artifactId>        <version>1.7</version>    </dependency>
```

3、对依赖项dubbo添加exclusion，避免引入旧spring

```
    <dependency>        <groupId>com.alibaba</groupId>        <artifactId>dubbo</artifactId>        <version>${project.parent.version}</version>        <exclusions>            <exclusion>                <groupId>org.springframework</groupId>                <artifactId>spring</artifactId>            </exclusion>        </exclusions>    </dependency>
```

4、webx已有spring 3以上的依赖，因此注释掉dubbo-admin里面的spring依赖

```
    <!--<dependency>-->        <!--<groupId>org.springframework</groupId>-->        <!--<artifactId>spring</artifactId>-->    <!--</dependency>-->
```



##  1.2、管理中心

![1569558082473](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1569558082473.png)

打包

![1569558109927](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1569558109927.png)

解压

![1569558166241](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1569558166241.png)点击运行

![1569558204001](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1569558204001.png)

# 三、总结---(步骤)

1、双击运行	D:\D\zookeeper\zookeeper-3.4.13\zookeeper-3.4.13\bin\zkServer.cmd	文件

2、双击运行	D:\D\zookeeper\zookeeper-3.4.13\zookeeper-3.4.13\bin\zkCli.cmd	文件

3‘、启动服务![1569570363282](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1569570363282.png)

4、浏览器输入	http://localhost:7001/	进行管理   账号:root  密码:root

5、双击运行	D:\D\zookeeper\dubbo-monitor-simple-2.0.0\assembly.bin\start.bat	文件

6、浏览器输入	http://localhost:8080/services.html	进行管理