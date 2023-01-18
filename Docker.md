# Docker

## Docker是什么

```
开发和运维 确保部署过程中不出现版本 配置问题

没有docker  搬家

有docker 搬楼 让环境迁移 完全一样 

一次镜像 处处运行

Docker 是解决了运行环境和配置问题的软件容器

方便做持续集成 并有助于整体发布的容器虚拟化技术
```



## 容器和虚拟机的比较

```
虚拟机 就是带环境安装的一种解决方案

缺点：

资源占用多 冗余步骤多  启动慢

Docker是在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统虚拟机则是在硬件层面实现虚拟化， Docker优势体现为 启动速度块，占用体积小
```



## 能干嘛

```
更快速的应用交付 和部署

更便捷的升级 和 扩缩容

更简单的系统运维

更高效的计算资源利用
```







## 去哪下

```
官网：docker.com

hub 官网：hub.docker.com
```



```
镜像文件

容器虚拟化技术

docker-hub 安装镜像的仓库


```



## Docker 的基本组成

### 镜像

```
类似于java中的类

就是一个**只读**的模板 镜像可以用来创建Docker容器 一个镜像可以创建很多个容器
```



### 容器 

```
类似于java中的实例对象
```

**容器是用镜像创建的运行实例**

```
它可以启动 开始 停止 删除

可以把容器看作是一个简易版的liunx环境
```



### 仓库

存放镜像的地方

![image-20230113163047945](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20230113163047945.png)

![image-20230114144338006](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20230114144338006.png)

​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

## 阿里云镜像加速器

这里使用腾讯云镜像

```shell
vim /etc/docker/daemon.json
```

```shell
{
   "registry-mirrors": [
       "https://mirror.ccs.tencentyun.com"
  ]
}
```

```shell
sudo systemctl restart docker
```

## Docker为什么快

```
1.有更少的抽象层

2.docker利用的是宿主机的内核 不需要假造操作系统的内核
```



## 命令

### 帮助启动类命令

![image-20230114213318225](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20230114213318225.png)

### 镜像命令

```shell
docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    feb5d9fea6a5   15 months ago   13.3kB

docker pull redis:6.0.8  // 带版本号的 拉取
docker serch
docker system df 查看镜像、容器所占的空间
docker rmi
```



#### Docker的虚悬镜像是什么

```
仓库名 标签名 都是null 的镜像
```



### 容器命令

有镜像才能创建容器

```shell
docker run --name= 为容器指定一个名字
-d 后台运行容器  并返回容器 ID  也即启动守护式容器

-i 交互模式运行容器 通常和-t同时使用
-t 为容器重新分配一个伪输入终端 和-i一起使用
也即启动交互式容器

-P 随机端口映射
-p 指定端口映射


docker ps 所有在运行的容器实例
docker ps -a 最近运行过的容器


exit 容器停止
ctrl + p + q 容器不停止

docker stop
docker rm
docker rm -f
docker ps -a -q


重要
好习惯
docker run -d 启动后台进程模式之后
docker ps 检查一下是否启动成功
因为 后台进程模式运行 docker前台没有运行的应用 这样的容器后台启动之后 会立即自杀


前台交互
docker run -it redis:6.0.8
后台守护
docker run -d redis:6.0.8

日志信息
docker logs 容器id

容器内部细节
docker inspect +id

重新进入
docker exec -it +id /bin/bash  直接进入容器启动命令的终端 不会启动新的进程 用exit不会导致容器停止   推荐使用exec
docker attach + id 直接进入容器启动命令的终端 不会启动新的进程 用exit会导致容器停止

从容器文件拷贝到主机
docker cp id :容器类路径 目的主机路径

导出容器
docker export id > 文件名.tar 导出容器
导入
cat abcd.tar | docker import - puc/ubuntu:3.7
```







## Docker镜像

![image-20230115225203510](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20230115225203510.png)

### UnionFS 

```
UnionFS 是一种分层的 轻量级的 且高性能的文件系统 支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下，是Docker镜像的基础 镜像可以通过分二层来继承，基于基础镜像，可以制作各种具体的应用镜像
```

```
rootfs
bootfs 
```

```
镜像分层的好处就是资源共享 为了复用
```

```
镜像层都是只读的 容器层才是可写的
当容器启动时，一个新的可写层别加载到镜像的顶部
这一次通常被称为“容器层” 之下的都叫“镜像层”
```

```
docker commmit 自己提交镜像
docker commit -m="add vim cmd" -a="puc" f6a885b36ac5 myubuntu:3.3
```



## 本地镜像推送到腾讯云

```
登录腾讯云容器镜像服务 Docker Registry
docker login ccr.ccs.tencentyun.com --username=100024163501
```

```
从 Registry 拉取镜像
docker pull ccr.ccs.tencentyun.com/pusg/myubuntu:[tag]
```

```
向 Registry 中推送镜像
docker tag [imageId] ccr.ccs.tencentyun.com/pusg/myubuntu:[tag]
docker push ccr.ccs.tencentyun.com/pusg/myubuntu:[tag]
```



## Docker私有库

```shell
curl -XGET localhost:5000/v2/_catalog

docker run -d -p 5000:5000 -v/puc/myregistry/:/tmp?registry --privileged=true registry

docker commit -m="ifconfig cmd add" -a="puc" + id pucubuntu:1.2

docker tag pucubuntu:1.2 127.0.0.1:5000/pucubuntu:1.2

vim /etc/docker/daemon.json
"insercure-registries":["127.0.0.1:5000"]   1.14.45.4 ???

systemctl restart docker

docker run -d -p 5000:5000 -v/puc/myregistry/:/tmp?registry --privileged=true registry

docker push 127.0.0.1:5000/pucubuntu:1.2

docker pull 127.0.0.1:5000/pucubuntu:1.2

```

## 容器数据卷

```
--privileged=true 放开权限 记得加
```

```
docker run -d -p 5000:5000 -v/puc/myregistry/:/tmp?registry --privileged=true registry
```

![image-20230116193947373](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20230116193947373.png)

```
卷就是目录或文件，存在于一个或多个容器中，由docker 挂载到容器，但是不属于联合文件系统，因此能绕过UFS 提供一些用于持续存储或共享数据的特性
卷的设计目的 就是 数据的持久化 完全独立于容器的生存周期，因此Docker 不会在容器删除时删除其挂在的数据卷
```

```
docker run -it --privileged=true -v/宿主机绝对路径目录:/容器内目录 镜像名
```

```
主机和容器双向的互通有无
```

```
只读  宿主机写的内容 容器可以读取到 但是 容器内只读
docker run -it --privileged=true -v/宿主机绝对路径目录:/容器内目录:ro 镜像名
```

```
卷的继承和共享

容器2 继承 容器1 的 容器卷
docker run -it --privileged=true --volumes -from u1 --name u2 ubuntu
```



## Docker常规安装简介

### tomcat

```
tomcat 10 有问题
webapps 文件夹是空的
要把这个文件夹删掉
mv webapps.dist webapps
```

```
直接使用tomcat8
docker run -d -p 8080:8080 --name mytomcat8 billygoo/tomcat8-jdk8
```



### mysql

```
简单版（不要用 没有备份 容器关闭之后容易造成数据丢失）
docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
还存在中文报错
```

```
新建mysql容器实例
docker run -d -p 3306:3306 --privileged=true 
-v /puc/mysql/log:/var/log/mysql 
-v /puc/mysql/data:/var/lib/mysql 
-v /puc/mysql/conf:/etc/mysql/conf.d 
-e MYSQL_ROOT_PASSWORD=123456  
--name mysql mysql:5.7
```

my.cnf

```
[client]
default_character_set=utf8
[mysqld]
collation_server = utf8_general_ci
character_set_server = utf8
然后重启mysql
```



### redis

```

```



# Docker高级篇

