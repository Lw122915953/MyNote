# 一、安装gcc-c++(基于CentOS01)

~~~java
yum -y install gcc-c++
~~~

![1573365391051](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573365391051.png)

# 二、安装libevent

~~~java
yum -y install libevent
~~~

![1573365524984](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573365524984.png)

# 三 、上传四个包至虚拟机

~~~java
#新建文件夹fastdfs在/usr/local路径下
cd /usr/local

mkdir fastdfs

FastDFS_v5.05.tar.gz
fastdfs-nginx-module_v1.16.tar.gz
libfastcommonV1.0.7.tar.gz
nginx-1.12.2.tar.gz
~~~

![1573365667407](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573365667407.png)

![1573365927199](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573365927199.png)

# 四、安装libfastcommon

~~~
tar -zxvf libfastcommonV1.0.7.tar.gz

#进入libfastcommon-1.0.7
#运行
./make.sh 

#如果无法运行./make.sh则先运行下列命令
yum -y install zlib zlib-devel pcre pcre-devel gcc gcc-c++ openssl openssl-devel libevent libevent-devel perl unzip net-tools wget 

运行
 ./make.sh install
 
#进入lib64
 cd /usr/lib64/

#复制文件到/usr/lib
cp libfastcommon.so /usr/lib/

~~~

![1573367780386](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573367780386.png)

![1573367886218](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573367886218.png)

![1573367846106](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573367846106.png)

![1573368132769](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573368132769.png)

# 五、安装FastDFS

~~~java
#进入路径
cd  /usr/local/fastdfs/

#解压
tar -zxvf FastDFS_v5.05.tar.gz 

#进入FastDFS
cd FastDFS

#运行
./make.sh

#安装
./make.sh install

#进入目录
/usr/local/fastdfs/FastDFS/conf

#复制
cp * /etc/fdfs/

    
~~~

![1573368421480](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573368421480.png)

![1573368448128](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573368448128.png)

![1573368527367](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573368527367.png)

![1573368861423](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573368861423.png)

# 六、安装Tracker服务

~~~java
#进入配置文件
vi /etc/fdfs/tracker.conf

#修改配置文件
base_path=/usr/local/fastdfs/FastDFS/tracker

#启动tracker
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf

~~~

![1573369090238](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573369090238.png)

![1573369065814](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573369065814.png)

# 七、安装Storage(存储节点)服务

~~~
#进入配置文件
vi /etc/fdfs/storage.conf

#修改配置文件的三个地方(定义文件放置路径)
base_path=/usr/local/fastdfs/FastDFS/storage
store_path0=/usr/local/fastdfs/FastDFS/storage
tracker_server=192.168.157.138:22122(本虚拟机的ip地址   用ifconfig查看   			22122是跟tracker.conf里的port相等)

#输入运行
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf

#进入文件夹复制文件
cd /usr/local/fastdfs/FastDFS/client/
cp libfdfsclient.so  /usr/lib/

#进入配置文件
vi /etc/fdfs/client.conf
#修改配置文件的两个地方
base_path=/usr/local/fastdfs/FastDFS/client
tracker_server=192.168.157.138:22122

#写个小文本测试(随便写)
vi /root/hi.html
#上传测试
/usr/bin/fdfs_test /etc/fdfs/client.conf upload /root/hi.html
~~~

![1573369576485](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573369576485.png)

![1573369679589](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573369679589.png)

![1573369776069](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573369776069.png)

![1573370601683](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573370601683.png)

# 八、配置防火墙

~~~java
#进入文件去配置过滤端口
vi /etc/sysconfig/iptables

#如果没有iptables就先执行下列五条语句
systemctl stop firewalld 
systemctl mask firewalld
yum install -y iptables 
yum install iptables-services
systemctl start iptables.service

#在iptables加入一条语句
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

#重启防火墙
systemctl restart iptables.service
~~~

![1573371435905](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573371435905.png)

# 九、安装Nginx和Nginx-fastdfs

~~~java
#下载
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel

#进入目录解压
cd /usr/local/fastdfs/
tar -zxvf fastdfs-nginx-module_v1.16.tar.gz

#进入目录
/usr/local/fastdfs/fastdfs-nginx-module/src

#修改配置文件
vi config

#在底行模式输入
%s/local\///g

#进入目录
/usr/local/fastdfs/fastdfs-nginx-module/src
#复制文件
cp mod_fastdfs.conf /etc/fdfs/
    
#配置文件(三个地方)
vi /etc/fdfs/mod_fastdfs.conf 
tracker_server=192.168.157.138:22122
store_path0=/usr/local/fastdfs/FastDFS/storage
url_have_group_name = true
~~~

![1573372104295](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573372104295.png)

![1573372369591](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573372369591.png)

![1573372462943](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573372462943.png)

![1573372608581](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573372608581.png)

![1573372642693](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573372642693.png)

![1573372752789](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573372752789.png)

# 十、安装配置Nginx

~~~
#进入目录
/usr/local/fastdfs
#解压
tar -zxvf nginx-1.12.2.tar.gz

#进入目录并复制这个目录
cd /usr/local/fastdfs/fastdfs-nginx-module/src

#进入目录并执行
cd /usr/local/fastdfs/nginx-1.12.2
#在这个目录执行
./configure --add-module=/usr/local/fastdfs/fastdfs-nginx-module/src

#再执行
make
make install

#进入目录
cd /usr/local/nginx/conf

#配置文件
vi nginx.conf

#在server中加入
 location /group1/M00/ {
                ngx_fastdfs_module;
        }

#进入目录
cd  /usr/local/nginx/sbin
#启动
./nginx

#可以在网页上访问了
~~~

![1573373042030](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573373042030.png)

![1573373611132](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573373611132.png)

# 十一、端口放行

~~~java
#进入配置文件
vi /etc/sysconfig/iptables
#加入下列语句
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22122 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 23000 -j ACCEPT
#重启防火墙
systemctl restart iptables.service
~~~



![1573374606562](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1573374606562.png)

# 十二、springboot集成

1. 创建springboot项目

2. 在pom.xml加入坐标

   ~~~
        <dependency>
               <groupId>com.github.tobato</groupId>
               <artifactId>fastdfs-client</artifactId>
               <version>1.26.1-RELEASE</version>
           </dependency>
   ~~~

   

3. 写application.yml

   ~~~yml
   fdfs:
     so-timeout: 1500
     connect-timeout: 600
     pool:
       jmx-enabled: false
     thumb-image:
       width: 100
       height: 100
     tracker-list: 192.168.157.138:22122
   ~~~

4. 测试

   ~~~java
   package com.bianyiit;
   
   import com.github.tobato.fastdfs.domain.StorePath;
   import com.github.tobato.fastdfs.service.FastFileStorageClient;
   import org.junit.jupiter.api.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.test.context.SpringBootTest;
   import org.springframework.test.context.junit4.SpringRunner;
   
   import java.io.File;
   import java.io.FileInputStream;
   
   @RunWith(SpringRunner.class)
   @SpringBootTest
   class Fastdfs01ApplicationTests {
       @Autowired
       private FastFileStorageClient fastFileStorageClient;
   
       @Test
       void contextLoads() {
           try {
               File file = new File("C:\\Users\\dell\\Desktop\\1573201810(1).jpg");
               String fileName = file.getName();
   
               System.out.println("fileName               " + fileName);
               String substring = fileName.substring(fileName.lastIndexOf(".") + 1);
               System.out.println("substring        " + substring);
               FileInputStream fileInputStream = new FileInputStream(file);
               StorePath storePath = fastFileStorageClient.uploadFile(fileInputStream, file.length(), substring, null);
               System.out.println("getGreoup          " + storePath.getGroup());
               System.out.println("getPath           " + storePath.getPath());
               System.out.println("getFullPath    " + storePath.getFullPath());
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   }
   ~~~

   ## 十三、

   ~~~java
   #查看存储位置
   store_path0=/usr/local/fastdfs/FastDFS/storage
   cd data
   cd 00
   cd 00
   ls
   #使用root用户运行
#运行
   /usr/bin/fdfs_storaged /etc/fdfs/storage.conf
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf
   
#进入目录
   cd  /usr/local/nginx/sbin
   #启动
   ./nginx
   
   #查看是否运行成功
   ps -ef | grep fdfs
   ps -ef | grep nginx
   ~~~
   
   
   
   
   
   