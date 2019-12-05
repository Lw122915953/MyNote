~~~java
#运行oracle容器
docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true --name oracle -v /dockerOracleData:/dockerOracleData  registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
#如果已经运行则先查看以运行容器得到其ID
docker ps -a
#然后启动
docker start 其ID
#进入容器
docker exec -it 其ID bash
#进入root
su root
#密码：helowin
#进入oracle
su - oracle
#进行软连接
sqlplus /nolog
#进入用户
conn system/system
#查看当前用户实例
show parameter instance_name;
~~~

~~~java
#移除已经停止的容器
docker rm 其ID
#移除正在运行的容器
docker rm -f 其ID
~~~

