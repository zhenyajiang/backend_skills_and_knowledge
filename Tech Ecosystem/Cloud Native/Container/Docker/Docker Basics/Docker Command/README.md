# Docker的命令很多，这里挑重点讲
阿里云镜像加速器
https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

## 镜像相关

##### 拉取镜像 <font color="maroon">docker pull</font>
```go
docker pull redis //拉取redis镜像

docker pull redis:latest //拉取redis最新版

docker pull ubuntu:14.04 //拉取ubuntu 14.04
```

##### 查看本地镜像 <font color="maroon">docker images </font>
```shell
$ docker images

REPOSITORY   TAG      IMAGE ID        CREATED      SIZE
debian       jessie   f50f9524513f    5 days ago   125.1 MB
debian       latest   f50f9524513f    5 days ago   125.1 MB
```

##### 基于dockerfile构建镜像 <font color="maroon">docker build </font>
```go
docker build -t 镜像名:<tags> .  
//-t 是给镜像命名
//. 代表基于当前目录的Dockerfile来构建镜像
```

##### 删除镜像 <font color="maroon">docker rmi <-f> 镜像名:\<tags\> </font>





## 容器相关
##### 查看正在运行中的容器 <font color="maroon">doker ps </font>