# Docker的命令很多，这里挑重点讲
阿里云镜像加速器
https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

#### 镜像相关

##### 拉取镜像 <font color="maroon">docker pull</font>
```go
docker pull redis //拉取redis镜像

docker pull redis:latest //拉取redis最新版

docker pull redis:6.0.5 //拉取redis6.0.5
```

##### 查看本地镜像 <font color="maroon">docker images </font>
```shell
$ docker images

REPOSITORY   TAG      IMAGE ID        CREATED      SIZE
debian       jessie   f50f9524513f    5 days ago   125.1 MB
debian       latest   f50f9524513f    5 days ago   125.1 MB
```

##### 构建镜像 <font color="maroon">docker build </font>
docker build -t minivision.com/sms_layer:1.0 .