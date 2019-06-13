# 前文链接

[Docker] 入门教程
https://www.jianshu.com/writer#/notebooks/20574865/notes/37511203


# 简介

> Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a Compose file to configure your application's services. Then, using a single command, you create and start all the services from your configuration.

> Compose是一个用于定义和运行多容器Docker应用程序的工具。使用组合，可以使用组合文件配置应用程序的服务。然后，使用单个命令从配置中创建和启动所有服务。

![](https://upload-images.jianshu.io/upload_images/2194379-74deec9ec05b634e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们从`logo`上可以看出来, 说白了, 这个东西就是一个管理容器的工(zhang)具(yu), 我们可以方便的使用它来管理我们的`docker`容器, 可以极大程度的简化命令行的复杂操作.



# 一.快速开始

如果你是`Mac`或`Windows`用户使用桌面版本的`Docker`应用默认就会替你安装`docker-compose`(下文中用dc代替), 如果是`centos`的话可以使用命令`yum -y install docker-compose`太简单了不做过多说明.

查看版本，能显示出来证明安装成功了

```
zhangyideMacBook-Pro:~ objcat$ docker-compose version
docker-compose version 1.23.1, build b02f1306
docker-py version: 3.5.0
CPython version: 3.6.6
OpenSSL version: OpenSSL 1.1.0h  27 Mar 2018
```

接下来我们有需求了， 运行一个`service-a`挂载到`/usr/local`, 映射端口为`8082`并添加`servicehost`域名映射内网`ip`，很多人可以想到那应该是一条冗长`docker`命令
```
docker run -idt -p 8082:8082 -v /Users/objcat/jar/service-a.jar:/usr/local/service-a.jar --name service-a java sh -c "echo 192.168.1.126 servicehost >> /etc/hosts && java -jar /usr/local/service-a.jar"
```
这么一大坨，看起来十分不方便，那么我们就是用`docker-compose`来优化一下

首先创建一个名为`docker-compose.yml`的文件
```
zhangyideMacBook-Pro:jar objcat$ touch docker-compose.yml
```

之后随便是用一个文本编辑器打开，写入下面内容

```
version: '2'
services:
 service-a: 
   image: java
   volumes:
     - /Users/objcat/jar/service-a.jar:/usr/local/service-a.jar
   ports:
     - 8082:8082
   command:
     - /bin/sh
     - -c
     - |
       echo 192.168.1.126 servicehost >> /etc/hosts
       java -jar /usr/local/service-a.jar
```

这样看起来是不是思路清晰多了呢，我们接下来运行一下 
```
zhangyideMacBook-Pro:jar objcat$ docker-compose up -d
Creating network "jar_default" with the default driver
Creating jar_service-a_1_495464b09e67 ... done
```
`-d`后台运行，否则运行log就会出现在你的屏幕上。。。

然后查看一下运行状态
```
zhangyideMacBook-Pro:jar objcat$ docker-compose ps
         Name                   Command           State           Ports         
--------------------------------------------------------------------------------
jar_service-             /bin/sh -c echo          Up      0.0.0.0:8082->8082/tcp
a_1_5827fa3204e8         192.168.1. ...   
```
我们可以看到服务已经运行起来了，我们试着访问一下
http://localhost:8082/hello

![](https://upload-images.jianshu.io/upload_images/2194379-dad905d0b4a0e1fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

成功，这一部分告一段落。

# 二.运行多个服务
有的人会问，多个服务怎么运行呢？很简单，我们这里就来做一下。

下面我要做的是开启三个服务`service-a`, `service-b`, `service-eureka`，分别是一个注册中心和两个服务。

我们来完善一下`docker-compose.yml`
```
version: '2'

services:

 service-eureka: 
   image: java
   volumes:
     - /Users/objcat/jar/service-eureka.jar:/usr/local/service-eureka.jar
   ports:
     - 8081:8081
   command:
     - /bin/sh
     - -c
     - |
       echo 192.168.1.126 servicehost >> /etc/hosts
       java -jar /usr/local/service-eureka.jar

 service-a: 
   image: java
   volumes:
     - /Users/objcat/jar/service-a.jar:/usr/local/service-a.jar
   ports:
     - 8082:8082
   command:
     - /bin/sh
     - -c
     - |
       echo 192.168.1.126 servicehost >> /etc/hosts
       java -jar /usr/local/service-a.jar

 service-b: 
   image: java
   volumes:
     - /Users/objcat/jar/service-b.jar:/usr/local/service-b.jar
   ports:
     - 8083:8083
   command:
     - /bin/sh
     - -c
     - |
       echo 192.168.1.126 servicehost >> /etc/hosts
       java -jar /usr/local/service-b.jar
```

我们来运行一下

```
zhangyideMacBook-Pro:jar objcat$ docker-compose up -d
jar_service-a_1_5827fa3204e8 is up-to-date
Creating jar_service-b_1_f7d01c317fab      ... done
Creating jar_service-eureka_1_57bb1a079e9b ... done
zhangyideMacBook-Pro:jar objcat$ docker-compose ps
              Name                             Command               State           Ports         
---------------------------------------------------------------------------------------------------
jar_service-a_1_5827fa3204e8        /bin/sh -c echo 192.168.1. ...   Up      0.0.0.0:8082->8082/tcp
jar_service-b_1_be87c0458e53        /bin/sh -c echo 192.168.1. ...   Up      0.0.0.0:8083->8083/tcp
jar_service-eureka_1_933d5a60af31   /bin/sh -c echo 192.168.1. ...   Up      0.0.0.0:8081->8081/tcp
zhangyideMacBook-Pro:jar objcat$ 
```

注册中心

![](https://upload-images.jianshu.io/upload_images/2194379-f018ea6fe45ac5a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

访问service-a

![](https://upload-images.jianshu.io/upload_images/2194379-461c3f4b8327e940.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

访问service-b

![](https://upload-images.jianshu.io/upload_images/2194379-627f69f065d37d4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到这里我们已经可以运行起来多个服务了！！！！下课 - -

## 命令扩充

> 停止服务
```
zhangyideMacBook-Pro:jar objcat$ docker-compose stop
Stopping jar_service-eureka_1_933d5a60af31 ... done
Stopping jar_service-b_1_be87c0458e53      ... done
Stopping jar_service-a_1_5827fa3204e8      ... done
```

> 停止单个服务
```
zhangyideMacBook-Pro:jar objcat$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
94c3472638c8        java                "/bin/sh -c 'echo 19…"   16 minutes ago      Up 6 seconds        0.0.0.0:8081->8081/tcp   jar_service-eureka_1_933d5a60af31
8d1dfa0ec642        java                "/bin/sh -c 'echo 19…"   16 minutes ago      Up 6 seconds        0.0.0.0:8083->8083/tcp   jar_service-b_1_be87c0458e53
6be5f9d8f423        java                "/bin/sh -c 'echo 19…"   33 minutes ago      Up 6 seconds        0.0.0.0:8082->8082/tcp   jar_service-a_1_5827fa3204e8
zhangyideMacBook-Pro:jar objcat$ docker stop 6be5f9d8f423
6be5f9d8f423
```

> 重新运行服务

```
zhangyideMacBook-Pro:jar objcat$ docker-compose up -d
jar_service-b_1_be87c0458e53 is up-to-date
Starting jar_service-a_1_5827fa3204e8 ... 
Starting jar_service-a_1_5827fa3204e8 ... done
zhangyideMacBook-Pro:jar objcat$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
94c3472638c8        java                "/bin/sh -c 'echo 19…"   19 minutes ago      Up 3 minutes        0.0.0.0:8081->8081/tcp   jar_service-eureka_1_933d5a60af31
8d1dfa0ec642        java                "/bin/sh -c 'echo 19…"   19 minutes ago      Up 3 minutes        0.0.0.0:8083->8083/tcp   jar_service-b_1_be87c0458e53
6be5f9d8f423        java                "/bin/sh -c 'echo 19…"   36 minutes ago      Up 13 seconds       0.0.0.0:8082->8082/tcp   jar_service-a_1_5827fa3204e8
```
我们可以看到`docker-compose`会自动识别容器的开启状态，替我们开启需要开启的那一个


# 三.内容补充

1.可能很多人对我的`echo 192.168.1.126 servicehost >> /etc/hosts`不是很理解

这里说一下，这句命令的意思是，把`servicehost`域名加入到`hosts`文件，目的是为了能让我的`eureka`发现我的服务，而不是把地址写死在配置文件中，这句命令对应的服务配置为
```
server:
  #服务端口号
  port: 8082
spring:
  application:
    #服务名称 - 服务之间使用名称进行通讯
    name: service-objcat-a
eureka:
  client:
    service-url:
      #填写注册中心服务器地址
      defaultZone: http://servicehost:8081/eureka
    #是否需要将自己注册到注册中心
    register-with-eureka: true
    #是否需要搜索服务信息
    fetch-registry: true
  instance:
    prefer-ip-address: true
    instance-id: ${spring.cloud.client.ip-address}:${server.port}
management:
  endpoints:
    web:
      exposure:
        include: "*"
```










 