# Nginx能做什么

- 反向代理
- 负载均衡
- HTTP服务器（动静分离）
- 正向代理

## 反向代理

反向代理应该是Nginx做的最多的一件事了，什么是反向代理呢，以下是百度百科的说法：反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。简单来说就是真实的服务器不能直接被外部网络访问，所以需要一台代理服务器，而代理服务器能被外部网络访问的同时又跟真实服务器在同一个网络环境，当然也可能是同一台服务器，端口不同而已。

## 负载均衡

负载均衡也是Nginx常用的一个功能，负载均衡其意思就是分摊到多个操作单元上进行执行，例如Web服务器、FTP服务器、企业关键应用服务器和其它关键任务服务器等，从而共同完成工作任务。简单而言就是当有2台或以上服务器时，根据规则随机的将请求分发到指定的服务器上处理，负载均衡配置一般都需要同时配置反向代理，通过反向代理跳转到负载均衡。而Nginx目前支持自带3种负载均衡策略，还有2种常用的第三方策略。

- rp(默认)

每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。

- 权重

指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。

- ip_hash

上面的2种方式都有一个问题，那就是下一个请求来的时候请求可能分发到另外一个服务器，当我们的程序不是无状态的时候（采用了session保存数据），这时候就有一个很大的很问题了，比如把登录信息保存到了session中，那么跳转到另外一台服务器的时候就需要重新登录了，所以很多时候我们需要一个客户只访问一个服务器，那么就需要用iphash了，iphash的每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

- fair（第三方）

按后端服务器的响应时间来分配请求，响应时间短的优先分配。

- url_hash（第三方）

按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。 在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法

## HTTP服务器（动静分离）

Nginx本身也是一个静态资源的服务器，当只有静态资源的时候，就可以使用Nginx来做服务器，同时现在也很流行动静分离，就可以通过Nginx来实现。

动静分离是让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，我们就可以根据静态资源的特点将其做缓存操作，这就是网站静态化处理的核心思路

## 正向代理

正向代理，意思是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端才能使用正向代理。当你需要把你的服务器作为代理服务器的时候，可以用Nginx来实现正向代理

---







# Nginx 安装前准备（没有虚拟机加QQ：2250847912）

> Nginx的安装需要Linux相关的几个库，否则配置和编译会出现错误，这几个库分别是：

- gcc编译器是否安装
  - 检查是否安装：`yum list installed | grep gcc`
  - 执行安装：`yum install gcc -y`
- openssl库是否安装
  - 检查是否安装：`yum list installed | grep openssl`
  - 执行安装：`yum install openssl openssl-devel -y`
- pcre库是否安装
  - 检查是否安装：`yum list installed | grep pcre`
  - 执行安装：`yum install prce prce-devel -y`
- zlib库是否安装
  - 检查是否安装：`yum list installed | grep zlib`
  - 执行安装：`yum installed zlib zlib-devel -y`
- 一次性安装，执行如下命令（推荐）
  - `yum install gcc openssl openssl-devel pcre-devel zlib zlib-devel -y`

# Nginx安装

> 菜鸟教程：https://www.runoob.com/linux/nginx-install-setup.html

- 在root目录创建一个soft文件夹，切换至soft目录

  - `mkdir soft`
  - `cd soft/`

- 上传文件

- `rz`（如果执行失败：` yum -y install lrzsz`）

- 解压,切换至解压安装包目录

  - `tar -zxvf nginx-1.1....`
  - `cd nginx-1.1....`

- 编译安装指定目录

  - `./configure --prefix=/usr/local/nginx`

    (--prefix是指定nginx安装路径 )

- 执行命名进行编译

- `make`

- 执行命名进行安装

  - `make install`（不行就执行：`make && make install`）

    





# 启动Nginx执行命令

（1）普通启动

- 切换到nginx安装目录的sbin目录下，执行：
  - `./ginx`

------

（2）通过配置文件启动

`/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf`

# 检查Nginx是否启动

`ps -ef | grep nginx`

> nginx体系结构由master进程和其worker进程组成；
>
> master进程读取配置文件，并维护worker进程，而worker进程则对请求进行实际处理





# Nginx的关闭多种方式

（1）缓慢关闭

- 找出nginx的进程号

- `ps -ef | grep nginx`

- 执行命名

  - `kill -QUIT 主pid`

    > 其中pid是主进程号的pid(master process)，其他为子进程pid(worker process)

------

（2）快速关闭

- 找出nginx的进程号
  - `ps -ef | grep nginx`
- 执行命名
  - `kill -TERM 主pid`（个人推荐：`kill 主pid`）

# 重启Nginx

- 切换到nginx安装目录的sbin目录下
  - `./nginx -s reload`





# 配置检查

> 当修改Nginx配置文件，可以使用Nginx命令进行配置文件语法检查，用于检查Nginx配置文件是否正确

- 检查Nginx配置文件是否正确
  - `/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf -t`

# 查看Nginx版本

`/usr/local/nginx/sbin/nginx -V`

> -v（小写v）显示nginx的版本
>
> -V（大写V）显示nginx的版本，编译器版本和配置参数









#定义Nginx运行的用户和用户组
#user www www;

#指定nginx进程数
#规则设定
#（1）cpu有多少个核，就有几位数，1代表内核开启，0代表内核关闭
#（2）worker_processes最多开启8个，8个以上性能就不会再提升了，而且稳定性会变的更低，因此8个进程够用了
worker_processes  1;

#配置Nginx多核CPU,worker_cpu_affinity使用方法和范例

#1. 2核CPU，开启2个进程


#worker_processes     2;
#worker_cpu_affinity 01 10;

#01表示启用第一个CPU内核，10表示启用第二个CPU内核
#worker_cpu_affinity 01 10;表示开启两个进程，第一个进程对应着第一个CPU内核，第二个进程对应着第二#个CPU内核。
#2. 2核CPU,开启4个进程

#worker_processes     4;
#worker_cpu_affinity 01 10 01 10;

#开启了四个进程，它们分别对应着开启2个CPU内核
#3. 4核CPU，开户4个进程

#worker_processes     4;
#worker_cpu_affinity 0001 0010 0100 1000;

#0001表示启用第一个CPU内核，0010表示启用第二个CPU内核，依此类推
#4. 4核CPU，开启2个进程

#worker_processes     2;
#worker_cpu_affinity 0101 1010;

#0101表示开启第一个和第三个内核，1010表示开启第二个和第四个内核
#2个进程对应着四个内核
#worker_cpu_affinity配置是写在/etc/nginx/nginx.conf里面的。
#2核是 01，四核是0001，8核是00000001，有多少个核，就有几位数，1表示该内核开启，0表示该内核关#闭。

#5. 8核CPU，开户8个进程

#worker_processes     8;
#worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;

#0001表示启用第一个CPU内核，0010表示启用第二个CPU内核，依此类推


#全局错误日志及PID文件
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

###===================================================

events {
    # 连接数上限
	# 配置每个worker进程连接数上限，nginx支持的总连接数就等于worker_processes*worker_connections
    worker_connections  1024;
}

###===================================================

### #设定http服务器，利用它的反向代理功能提供负载均衡支持

http {

    #设定mime类型,类型由mime.type文件定义
    include       mime.types;
    #默认文件类型
    default_type  application/octet-stream;    
    #设定日志格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    
    #使用哪种格式的日志
    #access_log  logs/access.log  main;
    
    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，对于普通应用，    
    sendfile        on;
    #防止网络阻塞
    #tcp_nopush     on;
    
    #连接超时时间（秒）
    #keepalive_timeout  0;
    keepalive_timeout  65;
    
    #开启gzip压缩
    #gzip  on;



    #设定负载均衡的服务器列表 支持多组的负载均衡,可以配置多个upstream  来服务于不同的Server.
    #nginx 的 upstream 支持 几 种方式的分配 
    #1)、轮询（默认） 每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。 
    #2)、weight 指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。 跟上面样，指定了权重。
    #3)、ip_hash 每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。 
    #4)、fair       
    #5)、url_hash #Urlhash
    upstream mysvr {
      #weigth参数表示权值，权值越高被分配到的几率越大   
      #1.down 表示单前的server暂时不参与负载
      #2.weight 默认为1.weight越大，负载的权重就越大。     
      #3.backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。  
      #server 192.168.1.116  down;
      #server 192.168.1.116  backup;
      server 192.168.1.121  weight=1;
      server 192.168.1.122  weight=2;
     #nginx的upstream目前支持4种方式的分配
        #1、轮询（默认）
        #每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
        #2、weight
        #指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。
        #例如：
        #upstream bakend {
        #    server 192.168.0.14 weight=10;
        #    server 192.168.0.15 weight=10;
        #}
        #2、ip_hash
        #每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
        #例如：
        #upstream bakend {
        #    ip_hash;
        #    server 192.168.0.14:88;
        #    server 192.168.0.15:80;
        #}
        #3、fair（第三方）
        #按后端服务器的响应时间来分配请求，响应时间短的优先分配。
        #upstream backend {
        #    server server1;
        #    server server2;
        #    fair;
        #}
        #4、url_hash（第三方）
        #按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
        #例：在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法
        #upstream backend {
        #    server squid1:3128;
        #    server squid2:3128;
        #    hash $request_uri;
        #    hash_method crc32;
        #}
    
        #tips:
        #upstream bakend{#定义负载均衡设备的Ip及设备状态}{
        #    ip_hash;
        #    server 127.0.0.1:9090 down;
        #    server 127.0.0.1:8080 weight=2;
        #    server 127.0.0.1:6060;
        #    server 127.0.0.1:7070 backup;
        #}
        #在需要使用负载均衡的server中增加 proxy_pass http://bakend/;
    
        #每个设备的状态设置为:
        #1.down表示单前的server暂时不参与负载
        #2.weight为weight越大，负载的权重就越大。
        #3.max_fails：允许请求失败的次数默认为1.当超过最大次数时，
        				返回proxy_next_upstream模块定义的错误
        
        #4.fail_timeout:max_fails次失败后，暂停的时间。
        #5.backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。
        			 所以这台机器压力会最轻。
    
        #nginx支持同时设置多组的负载均衡，用来给不用的server来使用。
        #client_body_in_file_only设置为On 可以讲client post过来的数据记录到文件中用来做debug
        #client_body_temp_path设置记录文件的目录 可以设置最多3层目录
        #location对URL进行匹配.可以进行重定向或者进行新的代理 负载均衡
    }
    
    #配置代理服务器的地址，即Nginx安装的服务器地址、监听端口、默认地址
    server {
        #1.侦听80端口 
        listen       80;
    
        #对于server_name,如果需要将多个域名的请求进行反向代理，
        可以配置多个server_name来满足要求
        
        server_name  localhost;		#配置服务名
        
        #charset koi8-r;	#配置字符集
    
        #access_log  logs/host.access.log  main;	#配置虚拟主句访问日志
    
    	#默认匹配斜杠/的请求，当访问路径中有斜杠/，会被该location匹配到并进行处理（ip+端口=root）
        location / {
            #root是配置服务器的默认网站根目录位置，默认主页目录在nginx安装目录的html子目录。
            root   html;
            index  index.html index.htm;     #配置首页文件的名称      
            proxy_pass http://mysvr; #跟载均衡服务器的upstream对应   
        }
    
        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        ## 定义错误提示页面
        #error_page   500 502 503 504  /50x.html;
        #location = /50x.html {
        #    root   html;
        #}
    
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}
    
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
    
    	#禁止访问.htaccess文件
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


	#配置另一个虚拟主机
	# another virtual host using mix of IP-, name-, and port-based configuration
	#
	#server {
	#    listen       8000;
	#    listen       somename:8080;
	#    server_name  somename  alias  another.alias;
	
	#    location / {
	#        root   html;
	#        index  index.html index.htm;
	#    }
	#}


	#配置https服务，安全的网络传输协议，加密传输
	# HTTPS server
	#
	#server {
	#    listen       443 ssl;
	#    server_name  localhost;
	
	#    ssl_certificate      cert.pem;
	#    ssl_certificate_key  cert.key;
	
	#    ssl_session_cache    shared:SSL:1m;
	#    ssl_session_timeout  5m;
	
	#    ssl_ciphers  HIGH:!aNULL:!MD5;
	#    ssl_prefer_server_ciphers  on;
	
	#    location / {
	#        root   html;
	#        index  index.html index.htm;
	#    }
	#}

}
 server {
        keepalive_requests 120; #单连接请求上限次数。
        listen       4545;   #监听端口
        server_name  127.0.0.1;   #监听地址       
        location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
           #root path;  #根目录
           #index vv.txt;  #设置默认页
           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
           deny 127.0.0.1;  #拒绝的ip
           allow 172.18.5.54; #允许的ip           
        }