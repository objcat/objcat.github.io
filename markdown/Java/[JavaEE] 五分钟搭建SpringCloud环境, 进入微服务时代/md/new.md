# 前言
**SpringCloud**并不是一个第三方框架的名称, 而是一整套微服务框架的统称, 使用这套框架可以快速搭建出高可用的微服务环境, 因为功能众多，所以又被称**SpringCloud全家桶**, 由于篇幅较长所以文章采用了目录引导, 第二章是微服务的基础, 第三章是功能模块扩展, 如网关, 服务保护, 分布式配置中心, nginx, 下面就跟着我们的文章, 一起来看吧.

# 目录
- 一.什么是微服务
- 二.快速开始
- 三.功能集成(网关, 服务保护, 负载均衡, 分布式配置中心)
- 四.打包
- 五.微服务发布利器 -- Docker
- 六.Demo

# 一.什么是微服务

> 就是把一整个后台项目拆分成多个模块, 每一个模块称作一个服务, 每个服务都可以独立运行, 这样做的好处是其中有一个服务挂掉后, 另外的服务不受影响, 这些服务使用接口相互通信, 减少了依赖和耦合.
-- 摘自白猫语录

# 二.快速开始

![](../img/1.png)

![](../img/2.png)

![](../img/3.png)

![](../img/4.png)

![](../img/5.png)

![](../img/6.png)

整个项目是由`maven`进行管理的, 依赖包就是我们开发时需要用到的第三方jar包, 也就是框架, 这里为什么什么也不选? 我说一下, 因为我们要做的是微服务, 所以框架结构为一个`基座`+`多个子模块`, 我们上面建立的就是`基座`, 你可以把它当成一个`工作空间`用途是管理子模块.


### 1.Eureka注册中心

首先要新建我们SpringCloud项目的核心`eureka注册中心`, 为什么说它是项目的核心呢, 我在简介中说过了, 微服务就是把一整个后台应用拆分成小的功能模块, 那么这些模块之间如何进行通信呢? 

没错就是注册中心, 那么我们接下来就开始搭建一个注册中心吧!

![](../img/7.png)

首先新建一个`Module`子项目, 也就是一个微服务模块.

![](../img/8.png)

![](../img/9.png)

![](../img/10.png)

![](../img/11.png)

到这里我们注册中心模块就建立好了, 之后我们来简单配置一下.

![](../img/12.png)

首先开启`eureka`服务

之后找到配置文件

![](../img/13.png)

配置一下`application.yml`, 如果你的是`application.properties`, 请修改后缀为`yml`, 这两个配置文件都可以配置工程, 相比之下`yml`更直观一些, 所以本教程使用`yml`进行配置

![](../img/14.png)

之后我们进行配置


```
server:
  # 配置服务端口
  port: 8081
eureka:
  client:
    service-url:
      # 配置eureka服务器地址
      defaultZone: http://127.0.0.1:8081/eureka
    #是否需要将自己注册到注册中心(注册中心集群需要设置为true)
    register-with-eureka: false
    #是否需要搜索服务信息 因为自己是注册中心所以为false
    fetch-registry: false
```

注意缩进, 因为`yml`使用缩进来区分不同字段的.

然后我们来运行项目吧!

![](../img/15.png)

在这里选择`ServiceEurekaApplication`, 然后点击绿色的运行即可.

![](../img/16.png)

看到上面的日志就证明成功了.

然后我们来访问一下注册中心吧.

http://localhost:8081

![](../img/17.png)

网页可以正常运行后 我们的注册中心就配置完毕了

## 2.创建微服务

只有注册中心是没有用的, 我们需要找一个地方来写接口, 所以接下来我们就创建两个服务.

![](../img/18.png)

![](../img/19.png)

接下来勾选一些功能模块

![](../img/20.png)

![](../img/21.png)

![](../img/22.png)

同理配置`service-b`, 新建完成之后是这个样子, 同理创建`service-b`

![](../img/23.png)

之后我们配置服务的入口文件

![](../img/24.png)


然后配置一下`application.yml`
首先是service-a
```
server:
  # 服务端口号
  port: 8082
spring:
  application:
    # 服务名称 - 服务之间使用名称进行通讯
    name: service-objcat-a
eureka:
  client:
    service-url:
      # 填写注册中心服务器地址
      defaultZone: http://localhost:8081/eureka
    # 是否需要将自己注册到注册中心
    register-with-eureka: true
    # 是否需要搜索服务信息
    fetch-registry: true
  instance:
    # 使用ip地址注册到注册中心
    prefer-ip-address: true
    # 注册中心列表中显示的状态参数
    instance-id: ${spring.cloud.client.ip-address}:${server.port}
```

之后是service-b

```
server:
  # 服务端口号
  port: 8083
spring:
  application:
    # 服务名称 - 服务之间使用名称进行通讯
    name: service-objcat-b
eureka:
  client:
    service-url:
      # 填写注册中心服务器地址
      defaultZone: http://localhost:8081/eureka
    # 是否需要将自己注册到注册中心
    register-with-eureka: true
    # 是否需要搜索服务信息
    fetch-registry: true
  instance:
    # 使用ip地址注册到注册中心
    prefer-ip-address: true
    # 注册中心列表中显示的状态参数
    instance-id: ${spring.cloud.client.ip-address}:${server.port}
```

之后我们来运行一下刚配置好服务吧

首先看编译器的左下角, 有一个`小方块`, 鼠标停靠在上面会显示菜单.

![](../img/25.png)

![](../img/26.png)

![](../img/27.png)

那我们接下来开始写个简单的接口

![](../img/28.png)

```
@RestController
public class TestController {
    @RequestMapping("/hello")
    public String hello() {
        return "hello world";
    }
}
```

![](../img/29.png)


重启服务, 访问下面地址

http://localhost:8082/hello

![](../img/30.png)

浏览器上出现`hello world`说明成功了

有人会问了 到这里并没有使用到任何注册中心的功能啊?
不要着急 接下来我们就是用一用注册中心
我们现在有一个需求 使用服务b调用服务a的接口
这时我们就需要用到`eurka(注册中心)`和`feign`客户端了
首先我们在service-b中创建`interface`

![](../img/31.png)

![](../img/32.png)

```
@FeignClient("SERVICE-OBJCAT-A")
public interface ServiceAFeignClient {
    @RequestMapping("/hello")
    public String hello();
}
```
应用名可以在eureka中找到
http://localhost:8081

![](../img/33.png)



之后我们来写接口 在服务b中添加个控制器

![](../img/34.png)

```
@RestController
public class TestController {

    @Autowired
    private ServerAFeignClient serverAFeignClient;

    @RequestMapping("/call")
    public String call() {
        String result = serverAFeignClient.hello();
        return "b to a 访问结果 ----- " + result;
    }
}
```

之后我们发现报错了 不要慌张设置一下即可

![](../img/35.png)


最后在应用入口加上注解，就能实现服务之间的调用了.

![](../img/36.png)

重新运行服务b 在网站上访问试试吧

[http://localhost:8083/call](http://localhost:8083/call)

![](../img/37.png)


到这里服务之间的相互访问也可以完成了 到这里springcloud最基本的环境搭建就完成了 快写几个微服务玩玩吧

## 内容扩展

我们来看一个神奇的现象 我把service-a接口中写一个延时函数 我们看一下效果

![](../img/38.png)

之后我们重新运行service-a

我们会发现报如下错误

![](../img/39.png)

意思是读取超时了 这是为什么呢

原因就是feign的默认请求超时时间是1秒 而我们延时1.5秒后返回了数据 所以这个请求失败了 那么我们要如何解决这个问题呢?

很简单 我们在配置文件中配置feign的超时时间即可
```
server:
  #服务端口号
  port: 8083
spring:
  application:
    #服务名称 - 服务之间使用名称进行通讯
    name: service-objcat-b
eureka:
  client:
    service-url:
      #填写注册中心服务器地址
      defaultZone: http://localhost:8081/eureka
    #是否需要将自己注册到注册中心
    register-with-eureka: true
    #是否需要搜索服务信息
    fetch-registry: true
ribbon:
  #建立连接超时时间
  ReadTimeout: 5000
  #读取资源超时间
  ConnectTimeout: 5000
```

关键是下面这三行
```
ribbon:
  #建立连接超时时间
  ReadTimeout: 5000
  #读取资源超时间
  ConnectTimeout: 5000
```


这里说一下 `ribbon` 如果你是搞IT的 那么你一定听说过一个叫`负载均衡`的东西 而负载均衡呢 又分为两种 
1.本地负载均衡 一般使用`ribbon`
2.服务器端负载均衡 一般使用`nginx`
顾名思义所谓`负载均衡`就是让用户访问可以平均到集群服务上去 避免单个服务访问量过大而增加服务器负担
所谓集群 就是单个服务在不同服务器上运行(相同服务器也可 但同一个服务器端口一定不同) 实现同样服务的效果

说到这里 你可能还是不太懂 没关系 先看下面的例子 我们来重现一下`ribbon`的负载`均衡功能`

我们现在就在本地开启两个service-a

首先修改一下代码 让服务打印出当前端口号

![](../img/40.png)

对服务a做了上图中的修改
1.获取服务器端口号
2.拼接端口号返回给客户端
3.注释掉了延时函数提高效率

好 那我们来启动两个service-a 这个要进行设置一下 因为idea默认情况下是单例运行的 我们先要给服务改成不是单例的状态 这样就能跑起来两个同样名称但端口不同的服务了

![](../img/41.png)

![](../img/42.png)

到了这一步 我们来跑起来两个service-a吧

![](../img/43.png)

![](../img/44.png)

运行之后我们会发现有两个a服务 一个是8082 一个是8092 

![](../img/45.png)

这个就成为集群 同一个服务开启了两个端口 独立运行 我们打开eureka看一下 服务都顺利运行了 

有两个a服务 一个b服务

![](../img/46.png)

接口下来我们用b服务来访问以下a服务 调用我们以前就写好的方法 `call`

![](../img/47.png)

访问的同一个地址 但是端口会在8082和8092之间来回切换 这就是所谓的本地负载均衡 那么这个机制是怎么实现的呢 其实很简单 就是轮询机制 计算公式就是 `总请求次数 % 服务器总数` 取模后就调用相对应索引的服务 就可以实现本地均衡负载了 这就是ribbon的基本原理

# 三.功能集成

## 1.网关
[JavaEE] SpringCloud集成Zuul网关
https://www.jianshu.com/p/6ef9ca1efa4b

## 2.服务保护
[JavaEE] SpringCloud集成Hystrix服务保护
https://www.jianshu.com/p/cce702d44b7d

## 3.负载均衡
[JavaEE] 狒狒都能懂的Nginx教程 for Mac
https://www.jianshu.com/p/c21606bb4044

## 4.分布式配置中心
[JavaEE] SpringCloud集成Apollo分布式配置中心
https://www.jianshu.com/p/5606483c7fbf
[JavaEE] SpringCloud集成SpringCloudConfig分布式配置中心
https://www.jianshu.com/p/f6c0793e8458

# 四.打包
[JavaEE] SpringCloud项目打包
https://www.jianshu.com/p/935868c9141e

# 五.微服务发布利器 -- Docker
[Docker] 入门教程
https://www.jianshu.com/p/7b3737df847d

# 六.Demo
https://github.com/objcat/test-spring-cloud-demo.git

# finall enjoy it.
# by objcat 2018.11.14

更新日志:
- 2018年11月20日14:24:58
新增负载均衡原理
新增网关配置教程
- 2018年12月14日15:38:58
重构文章, 完善部分细节
- 2018年12月21日15:25:46
完善Demo, 新增Zuul网关和分布式配置中心Config
- 2018年12月25日11:35:33
完善目录和部分词语修正


