# 一、安装

这里提供apt-get快速安装的方法：
执行安装命令

```
sudo apt-get install mysql-server
```

![Ubuntu安装MYSQL](https://s1.51cto.com/images/blog/201812/26/d9dc4fcccda8268e9966d24d96bb038f.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

这里会提示输入root的密码，输入自己的密码即可。如果这里你们有输入密码，不要慌张，后面有补救的方法。

![Ubuntu安装MYSQL](https://s1.51cto.com/images/blog/201812/26/3cdafa0bf6ca504a3be95d3845262e4a.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

![Ubuntu安装MYSQL](https://s1.51cto.com/images/blog/201812/26/294f71546ef635a3528c05b6619f38a7.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
到这里就可以配置mysql了：

```
sudo mysql_secure_installation
```

因为之前设置过root密码了，所以这里会提示让你输入密码验证，下面有一步骤是重置root密码，你可以重置也可以不重置，看自己心情。

![Ubuntu安装MYSQL](https://s1.51cto.com/images/blog/201812/26/2e1d19a83e6e228c372e00bbc8e62d3d.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

![Ubuntu安装MYSQL](https://s1.51cto.com/images/blog/201812/26/be1fea6cc0e3197f66c8616b909420dd.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

查看状态

```
#检查mysql是不是在运行
sudo service mysql status

#一般安装完成之后都是会自动运行的，如果没有运行可以start
sudo service mysql start
```

然后到这里就可以直接访问了

```
sudo mysql -uroot -p
```

如果之前没有输入过密码，就没办法配置成功。
而且使用mysql -uroot -p 命令连接mysql时，报错

```
ERROR 1045 (28000): Access denied for user 'root'@'localhost' 
```

此时修改root的默认密码即可
进入到etc/mysql 目录下，查看debian.cnf文件
![Ubuntu安装MYSQL](https://s1.51cto.com/images/blog/201812/26/e5e6fa996be491d28fa9e24f3fa9473b.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

找到用户名，密码 ，使用此账号登录mysql
用户名：debian-sys-maint
密码：xedvSNKdLavjuEWV
登录：

```
mysql -udebian-sys-maint -p
```

修改root用户的的密码

```
show databases；
use mysql;
update user set authentication_string=PASSWORD("自定义密码") where user='root';
update user set plugin="mysql_native_password"; 
flush privileges;
quit;
```

修改完密码，需要重启mysql

```
/etc/init.d/mysql restart;
```

然后再登录

```
mysql -u root -p
```

附上卸载命令:

```
sudo apt purge mysql-*
sudo rm -rf /etc/mysql/ /var/lib/mysql
sudo apt autoremove
```

# 二、让navicat连接

1、启动虚拟机后，使用Xshell连接到虚拟机，登录数据库服务，创建一个test用户并授权；



![img](https:////upload-images.jianshu.io/upload_images/4650326-00ada8dac253ac81.png?imageMogr2/auto-orient/strip|imageView2/2/w/766/format/webp)



2、创建用户之后，刷新权限；

flush privileges

3、重启数据库服务

sudo service mysql restart

4、cd /etc/mysql 进入到my.cnf文件所在的目录下，sudo cp my.cnf my.cnf.bak，备份文件



![img](https:////upload-images.jianshu.io/upload_images/4650326-1c39ecc3faefbeaa.png?imageMogr2/auto-orient/strip|imageView2/2/w/481/format/webp)



5、打开配置，找到bind-address= 127.0.0.1这一行，注释掉





![img](https:////upload-images.jianshu.io/upload_images/4650326-bd4b038aa6a1996b.png?imageMogr2/auto-orient/strip|imageView2/2/w/642/format/webp)



6、重启数据库，使用Navicat进行连接。



![img](https:////upload-images.jianshu.io/upload_images/4650326-b5f8fa6197c01c50.png?imageMogr2/auto-orient/strip|imageView2/2/w/924/format/webp)

* 修改bind-address

在服务器上部署mysql，想让本地navicat远程连接，但是明明已经赋予权限了，却死活连接不上，看别人教程说是要在/etc/mysql/my.cnf中将“bind-address”给注释掉，结果发现里面什么都没有
最后发现原来在/etc/mysql/mysql.conf.d/mysqld.cnf中
注释完重启一下/etc/init.d/mysql restart
然后再来看看3306端口的监听情况：netstat -an | grep 3306
就会发现没有监听“127.0.0.1:3306”了

