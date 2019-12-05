# Docker中使用MySQL

2018-08-22 14:11:19  更多



版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。本文链接：https://blog.csdn.net/ishellhub/article/details/81943169

原文地址

> https://github.com/shellhub/blog/issues/21

## 安装Docker

Linux或者Mac用户建议使用一件脚本安装

https://github.com/docker/docker-install

Windows用户(好久没用这个系统了)

https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install

## 进入主题

首先下载使用dockek下载mysql镜像

```bash
# download mysql from docker library
docker pull mysql12
```

启动mysql服务器

```bash
# mysql给该容器搞一个标识
# 3306:3306映射容器端口到本机端口
# root为数据库密码
# latest为mysql版本，此处表示最新版本，可以不填写
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql:latest12345
```

进入mysql容器

```bash
docker exec -it mysql bash1
```

登录mysql

```bash
# root为数据库密码，这儿显示输入
mysql -u root -proot12
```

## 例子

```bash
# 显示数据库
show databases;
# 创建数据库
create database db_example;1234
```

## 总结

~~~java
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql:5.7
    
docker exec -it mysql bash

mysql -u root -proot
~~~

