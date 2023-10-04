# 🍎 Gateway

## 🌲 创建微服务

![](images/Pasted%20image%2020231003162042.png)

结构是这样的

![](images/Pasted%20image%2020231003163828.png)

和普通微服务没区别, 再说吐了

## 🌲 配置依赖

我们的`gateway`也需要依赖`common`, 另外需要一个网关`gateway`

```xml
<dependencies>  
    <dependency>  
        <groupId>com.objcat</groupId>  
        <artifactId>gulimall-api-common</artifactId>  
        <version>1.0</version>  
    </dependency>  
  
    <dependency>  
        <groupId>org.springframework.cloud</groupId>  
        <artifactId>spring-cloud-starter-gateway</artifactId>  
    </dependency>  
</dependencies>
```

## 🌲 配置文件

视频中把配置文件写在了分布式配置中心, 我觉得没必要, 直接写在本地即可

- application.yml

```yml
server:  
  # 服务端口号  
  port: 88  
  servlet:  
    encoding:  
      # 返回数据使用utf-8编码  
      charset: utf-8  
      # 强制使用utf-8, 否则某些浏览器中查看会乱码  
      force: true  
  
spring:  
  datasource:  
    # 数据库连接url  
    url: 1  
    # 驱动类名  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    # 数据库用户名  
    username: root  
    # 数据库密码  
    password: 123456
```

- bootstrap.yml

```yml
spring:  
  application:  
    # 配置服务的名称  
    name: gulimall-gateway  
  cloud:  
    nacos:  
      discovery:  
        # 配置nacos服务端地址  
        server-addr: 127.0.0.1:8848
```

因为`gateway`不需要配置数据库, 所以`url`我就写了一个`1`占位

## 🌲 启动服务

我们会发现一个错误

```
***************************
APPLICATION FAILED TO START
***************************

Description:

Spring MVC found on classpath, which is incompatible with Spring Cloud Gateway.

Action:

Please set spring.main.web-application-type=reactive or remove spring-boot-starter-web dependency.
```

解决方案它告诉你了

```yml
spring:  
  main:  
    web-application-type: reactive
```

配置完重新启动, 不报错就是对了

## 🌲 转发

我们下面就来尝试一下网关转发到其他网站吧

```
spring:  
  cloud:  
    gateway:  
      routes:  
        - id: baidu  
          uri: https://www.baidu.com  
          predicates:  
            - Query=url,baidu
```

比如转发到百度

http://localhost:88/?url=baidu

http://localhost:88/?url=qq

只要我们的`url=baidu`那么网关就会给我们转发到百度去, 如果是`url=qq`就会转发到qq去
