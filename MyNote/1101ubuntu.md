# 一：命令

## 1.打包解包

~~~java
# 打包文件
$ tar -cvf 打包文件.tar 被打包的文件/路径...
 
# 解包文件
$ tar -xvf 打包文件.tar
~~~

| 选项 | 含义                                                         |
| ---- | :----------------------------------------------------------- |
| c    | 生成档案文件，创建打包文件                                   |
| x    | 解开档案文件                                                 |
| v    | 列出归档解档的详细过程，显示进度                             |
| f    | 指定档案文件名称，f 后面一定是 .tar 文件，**所以必须放选项最后** |
| z    | 调用 gzip实现压缩和解压缩的功能                              |

## 2.压缩解压缩

* 2.1 gzip 

  tar 与 gzip 命令结合可以实现文件 打包和压缩，tar 只负责打包文件，但不压缩，用 gzip 压缩 tar 打包后的文件，其扩展名一般用 xxx.tar.gz
         在Linux中，最常见的压缩文件格式就是 xxx.tar.gz

  在 tar 命令中有一个选项 -z 可以调用 gzip，从而可以方便的实现压缩和解压缩的功能
  命令格式如下：

  ~~~java
  # 压缩文件
  $ tar -zcvf 打包文件.tar.gz 被压缩的文件/路径...
   
  # 解压缩文件
  $ tar -zxvf 打包文件.tar.gz
   
  # 解压缩到指定路径，注意：要解压的目录必须存在
  $ tar -zxvf 打包文件.tar.gz -C 目标路径
  ~~~

  

* 2.2 bzip2

  tar 与 bzip2 命令结合可以实现文件 打包和压缩 （用法和gzip一样），tar 只负责打包文件，但不压缩，用 bzip2 压缩 tar 打包后文件，其扩展名一般用 xxx.tar.bz2
  在 tar 命令中有一个选项 -j 可以调用 bzip2，从而可以方便的显示压缩和解压缩的功能
  命令格式如下

~~~java
# 压缩文件
$ tar -jcvf 打包文件.tar.bz2 被压缩的文件/路径...
 
# 解压缩文件
$ tar -jxvf 打包文件.tar.bz2
~~~

## 3.管道流

~~~java
#查找后缀有 file 字样的文件中包含 test 字符串的文件，并打印出该字符串的行
grep test *file 
~~~

## 4.查看安装的软件

~~~java
#查看所有安装的软件
rpm -qa 

#模糊查询带有规定字符的软件
rpm -qa | grep xxx
~~~

## 5.查看进程

~~~java
#查看所有进程
ps aux

#使用管道流操作筛选带有规定字符的进程
ps aux | grep xxx

~~~

## 6.删除

~~~java
#Linux 删除文件夹和文件的命令
#-r 就是向下递归，不管有多少级目录，一并删除
#-f 就是直接强行删除，不作任何提示的意思

#删除文件夹实例：
rm -rf /var/log/httpd/access
#将会删除/var/log/httpd/access目录以及其下所有文件、文件夹

#删除文件使用实例：
rm -f /var/log/httpd/access.log
#将会强制删除/var/log/httpd/access.log这个文件
~~~

## 7.查询文件

~~~java
#linux怎么模糊查找一个文件
#linux如何模糊查找一个文件

#在当前目录下搜索指定文件：
find . -name test.txt

#在当前目录下模糊搜索文件：
find . -name '*.txt'

#在当前目录下搜索特定属性的文件：
find . -amin -10 # 查找在系统中最后10分钟访问的文件
find . -atime -2 # 查找在系统中最后48小时访问的文件
find . -empty # 查找在系统中为空的文件或者文件夹
find . -group cat # 查找在系统中属于 groupcat的文件
find . -mmin -5 # 查找在系统中最后5分钟里修改过的文件
find . -mtime -1 #查找在系统中最后24小时里修改过的文件
find . -nouser #查找在系统中属于作废用户的文件
find . -user fred #查找在系统中属于FRED这个用户的文件

#在当前目录搜索文件内容含有某字符串（大小写敏感）的文件：
find . -type f | xargs grep 'your_string'

#在当前目录搜索文件内容含有某字符串（大小写敏感）的特定文件：
find . -type f -name '*.sh' | xargs grep 'your_string'

#在当前目录搜索文件内容含有某字符串（忽略大小写）的特定文件：
find . -type f -name '*.sh' | xargs grep -i 'your_string'
~~~

## 8.权限操作

~~~java
#r:read就是读权限     --数字4表示
#w:write就是写权限    --数字2表示
#x:excute就是执行权限 --数字1表示

sudo chmod 777 file
~~~

