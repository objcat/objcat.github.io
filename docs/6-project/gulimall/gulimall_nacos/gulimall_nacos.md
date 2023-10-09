# 🍎 Nacos搭建

`nacos`在`spring-cloud`中充当发现中心语配置中心角色, 首先你要去安装服务端 [跳转 nacos_env](../../3-program/env/nacos/nacos_env.md)

## 🌲 启动nacos

```
sh startup.sh -m standalone
```

启动后自己访问一下

http://localhost:8848/nacos

## 🌲 导入依赖

然后我们要导入依赖, 直接配置到通用模块

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>

<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

我们可以看到除了`discovery`, 我们导入一个`bootstrap`, 注意你如果按照我的步骤来, 就必须开启`bootstrap`

## 🌲 配置

我们在本地创建`boostrap.yml`然后写入配置, 这个配置写在`application.yml`中也可以, 但是为了以后配置中心方便, 我们提前创建这个配置文件

```yml
spring:
  application:
    # 配置服务的名称
    name: gulimall-product
  cloud:
    nacos:
      discovery:
        # 配置nacos服务端地址
        server-addr: 127.0.0.1:8848
```

## 🌲 测试

然后我们启动项目, 看一下服务列表

![](images/Pasted%20image%2020230927171503.png)

# 🍎 openfeign

## 🌲 生成代码

然后视频带我们去做了`openfeign`的跨服务通讯, 内容是`member用户服务`调用`coupon优惠券服务`, 所以我们先把这两个服务的代码生成一下, 改动配置如下

- member

```
url: jdbc:mysql://localhost:3306/gulimall_ums?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai

# 主目录
mainPath=com.objcat
# 包名
package=com.objcat
moduleName=member
# 作者
author=objcat
# Email
email=objcat2024@gmail.com
# 表前缀(类名不会包含表前缀)
tablePrefix=ums_
```

- coupon

```
url: jdbc:mysql://localhost:3306/gulimall_sms?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai

# 主目录
mainPath=com.objcat
# 包名
package=com.objcat
moduleName=coupon
# 作者
author=objcat
# Email
email=objcat2024@gmail.com
# 表前缀(类名不会包含表前缀)
tablePrefix=sms_
```

在粘贴文件夹的过程中有可能出现错误

![](images/Pasted%20image%2020230928171032.png)

我的解法是直接`show in finder`在文件目录去粘贴, 不要去`idea`中粘贴

## 🌲 创建主启动类并配置项目

然后我们来创建这两个服务的主启动文件和配置文件, 主启动文件

```java
@SpringBootApplication
public class CouponApplication {
    public static void main(String[] args) {
        SpringApplication.run(CouponApplication.class, args);
    }
}

@SpringBootApplication
public class MemberApplication {
    public static void main(String[] args) {
        SpringApplication.run(MemberApplication.class, args);
    }
}
```

配置文件

- member

```yml
server:
  # 服务端口号
  port: 8000
  servlet:
    encoding:
      # 返回数据使用utf-8编码
      charset: utf-8
      # 强制使用utf-8, 否则某些浏览器中查看会乱码
      force: true

spring:
  datasource:
    # 数据库连接url
    url: jdbc:mysql://localhost:3306/gulimall_ums
    # 驱动类名
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 数据库用户名
    username: root
    # 数据库密码
    password: 123456

mybatis-plus:
  # 配置mapper存放位置
  mapper-locations: classpath:mapper/**/*.xml
  # 配置日志打印策略
  configuration:
    log-impl: org.apache.ibatis.logging.nologging.NoLoggingImpl
  # 配置主键生成策略
  global-config:
    db-config:
      id-type: auto
```

bootstrap.yml

```yml
spring:
  application:
    # 配置服务的名称
    name: gulimall-member
  cloud:
    nacos:
      discovery:
        # 配置nacos服务端地址
        server-addr: 127.0.0.1:8848
```

- coupon

```yml
server:
  # 服务端口号
  port: 7000
  servlet:
    encoding:
      # 返回数据使用utf-8编码
      charset: utf-8
      # 强制使用utf-8, 否则某些浏览器中查看会乱码
      force: true

spring:
  datasource:
    # 数据库连接url
    url: jdbc:mysql://localhost:3306/gulimall_sms
    # 驱动类名
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 数据库用户名
    username: root
    # 数据库密码
    password: 123456

mybatis-plus:
  # 配置mapper存放位置
  mapper-locations: classpath:mapper/**/*.xml
  # 配置日志打印策略
  configuration:
    log-impl: org.apache.ibatis.logging.nologging.NoLoggingImpl
  # 配置主键生成策略
  global-config:
    db-config:
      id-type: auto
```

bootstrap.yml

```yml
spring:
  application:
    # 配置服务的名称
    name: gulimall-coupon
  cloud:
    nacos:
      discovery:
        # 配置nacos服务端地址
        server-addr: 127.0.0.1:8848
```

8000和7000端口是按照视频上面配置的, 但是有些时候你可能会发现端口占用, 比如m1苹果上面7000端口就被占用了, 用下面的命令可以查看端口占用

```
lsof -i :7000

COMMAND    PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME

ControlCe 2154 objcat    5u  IPv4 0xced43dc87b1820ff      0t0  TCP *:afs3-fileserver (LISTEN)

ControlCe 2154 objcat    6u  IPv6 0xced43dcd47761bd7      0t0  TCP *:afs3-fileserver (LISTEN)
```

你会发现你永远启动不起来程序, 所以这个时候我们就要去换个端口, 比如7001

## 🌲 测试

配置完在`nacos`看到这样的画面就是成功注册了

![](images/Pasted%20image%2020230928173543.png)

## 🌲 写获取优惠券接口

视频中的要求是`member`调用`coupon`来获取优惠券, 但是由于我们现在库里连用户都没有更别说优惠券了, 所以只能造假的数据, 下面先来写一个获取优惠券列表的接口

```java
@RestController
@RequestMapping("coupon/coupon")
public class CouponController {

    @Autowired
    private CouponService couponService;

    @RequestMapping("/listCoupon")
    public R listCoupon() {
        CouponEntity couponEntity = new CouponEntity();
        couponEntity.setCouponName("满100减10");
        return R.ok().put("data", Arrays.asList(couponEntity));
    }
}
```

然后重启访问

http://localhost:7000/coupon/coupon/listCoupon

然后可以看到返回结果

```json
{"msg":"success","code":0,"data":[{"id":null,"couponType":null,"couponImg":null,"couponName":"满100减10","num":null,"amount":null,"perLimit":null,"minPoint":null,"startTime":null,"endTime":null,"useType":null,"note":null,"publishCount":null,"useCount":null,"receiveCount":null,"enableStartTime":null,"enableEndTime":null,"code":null,"memberLevel":null,"publish":null}]}
```

看到上面返回的json说明优惠券请求成功了

## 🌲 调用服务

然后我们在`member`中写接口调用这个服务, 实现服务之间的通讯, 需要的依赖就是`openfeign`, 我们在第一期已经配置过了, 我们下面就看看要怎么使用`openfeign`吧

首先要创建一个`feignService`接口, 比如

```java
@FeignClient("gulimall-coupon")
public interface CouponFeignService {
    @RequestMapping("/coupon/coupon/listCoupon")
    public R listCoupon();
}
```

然后我们在主启动类中开启`openfeign`

```java
@EnableFeignClients(basePackages = "com.objcat.member.feign")
@SpringBootApplication
public class MemberApplication {
    public static void main(String[] args) {
        SpringApplication.run(MemberApplication.class, args);
    }
}
```

然后我们写接口

```java
@RestController
@RequestMapping("member/member")
public class MemberController {
    @Autowired
    private MemberService memberService;

    @Autowired
    private CouponFeignService couponFeignService;

    @RequestMapping("/listCoupon")
    public R listCoupon() {
        return couponFeignService.listCoupon();
    }
}
```

然后我们启动程序, 如果你使用的是我的版本你会遇到这样一个错误

```
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'memberController': Unsatisfied dependency expressed through field 'couponFeignService'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.objcat.member.feign.CouponFeignService': Unexpected exception during bean creation; nested exception is java.lang.IllegalStateException: No Feign Client for loadBalancing defined. Did you forget to include spring-cloud-starter-loadbalancer?
```

我们需要导入一个依赖在`common`中

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-loadbalancer</artifactId>
</dependency>
```

然后再次启动程序错误解决, 如果你和视频中用的一个版本可能没有问题, 启动程序后我们访问一下接口

http://localhost:8000/member/member/listCoupon

发现可以获取优惠券了

```json
{"msg":"success","code":0,"coupons":[{"id":null,"couponType":null,"couponImg":null,"couponName":"满100减10","num":null,"amount":null,"perLimit":null,"minPoint":null,"startTime":null,"endTime":null,"useType":null,"note":null,"publishCount":null,"useCount":null,"receiveCount":null,"enableStartTime":null,"enableEndTime":null,"code":null,"memberLevel":null,"publish":null}]}
```

# 🍎 配置中心

接下来视频中讲了配置中心, 我就白话来说了, 有一些配置是要经常去修改的, 比如圣诞结活动开启的配置, 为true开始圣诞狂欢, 为false结束圣诞活动, 但是每次更改都重新发布那么多微服务这是很麻烦的, 所以人们想了一个办法, 就是使用配置中心来配置, 有一个好处就是修改配置后会同步到微服务上, 这样就可以灵活的来控制配置了, 那我们下面就一起来看看吧

## 🌲 导入依赖

```xml
<dependency>
	<groupId>com.alibaba.cloud</groupId>
	<artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

## 🌲 配置

注意是`bootstrap.yml`

```yml
spring:  
  application:  
    # 配置服务的名称  
    name: gulimall-coupon  
  cloud:  
    nacos:  
      discovery:  
        # 配置nacos服务端地址  
        server-addr: 127.0.0.1:8848  
      config:  
        # 配置中心服务器地址  
        server-addr: 127.0.0.1:8848  
        # 配置文件扩展名  
        file-extension: yaml  
```

可以发现我这里和视频配置的不同, 视频没有配置`file-extension`, 那么后缀就使用默认的`properties`, 而我喜欢yml所以配置文件使用yaml来写

## 🌲 创建默认配置

我们为了方便测试, 先在本地写死一个默认配置, 在`application.yml`中

```
coupon:  
  user:  
    name: "张三"  
    age: 18
```

## 🌲 创建接口

我们创建接口去读取配置, 跟视频一样在`CouponController`中去写

```java
@Value("${coupon.user.name}")  
private String name;  
  
@Value("${coupon.user.age}")  
private String age;  
  
@RequestMapping("/test")  
public R test() {  
    return R.ok().put("data", name + " " + age);  
}
```

我们使用`@Value`来读取本地配置

## 🌲 测试

我们访问接口试试

http://localhost:7001/coupon/coupon/test

可以看到是能够读取的, 注意端口号别错了, 我是因为7000端口被占用了, 所以改了别的, 能看到这一串说明读出来了

```
{"msg":"success","code":0,"data":"张三 18"}
```

## 🌲 新建配置文件

我们下面就使用配置中心来配置吧, 首先点击新建

![](images/Pasted%20image%2020230929115438.png)

然后写一个配置, 名字是`应用名.yaml`, 比如我的就是`gulimall-coupon.yaml`

![](images/Pasted%20image%2020230929115853.png)

重点来了, 配置文件中我把name改成了李四, age改成了20, 这样方便区分

然后点发布, 我们重启项目访问接口看看效果

```
{"msg":"success","code":0,"data":"李四 20"}
```

可以看到变成了`李四 20`, 这说明配置生效了, 也同样说明另外一个特性, 就是覆盖性, 我们本地的配置只是默认的, 如果配置中心和本地的配置重复就会出现覆盖

## 🌲 配置刷新

我们会发现一个问题, 就是当我们修改完配置文件后, 从新访问接口, 如果不重启应用, 配置文件是不会改变了, 这与我们的期待不符合, 所以我们需要让配置实时生效, 使用`@RefreshScope`注解

```java
@RefreshScope  
@RestController  
@RequestMapping("coupon/coupon")  
public class CouponController {  
  
    @Autowired  
    private CouponService couponService;  
  
    @Value("${coupon.user.name}")  
    private String name;  
  
    @Value("${coupon.user.age}")  
    private String age;  
  
    @RequestMapping("/test")  
    public R test() {  
        return R.ok().put("data", name + " " + age);  
    }
}
```

完活, 这回配置修改后访问接口就是实时生效的

# 🍎 配置区分

当你的微服务足够多, 或者环境足够多的时候, 你会发现这种单一的配置(一个服务一个配置文件)可能无法满足你, 因为实际项目中需要区分`环境`有`生产开发测试环境`等, 视频第24集讲的真是「又长又一头雾水」听起来有点烦躁, 我并不是说讲的做不通, 照着配都能配出来, 但实际工作中怎么运用讲的很少, 下面我就以我的经验分别来写这几个模块

# 🍎 DataId区分

`DataId`顾名思义就是数据的`Id`, 我们在配置文件的时候配置的第一个参数「文件名」就是`DataId`, 但是`DataId`也有一个区分环境的功能, 视频里没讲, 所以我这里准备给大家补上, 想学区分环境, 我们就要先学习在`springboot`中如何区分环境, 怎么区分呢? 其实很简单哈, 再次强调这章是我新加的

## 🌲 创建配置文件

首先除了你现在有的`bootstrap.yml`应该再创建出两个`bootstrap-dev.yml和bootstrap-prod.yml`分别代表开发环境和生产环境, 然后创建出两个`application-dev.yml和application-prod.yml`这俩也是配置文件, 也是咱们每个工程中基本都要用的, 创建好后开始写内容

- bootstrap.yml

主配置文件, 用来切换环境`spring.profiles.active`改成`dev`就是开发环境, 改成`prod`就是生产环境, 它可以控制所有配置文件的选择, 包括`applicationxxx.yml`, 你可以理解为总开关

```yml
spring:  
  profiles:  
    active: dev
```

- bootstrap-dev.yml

直接把`bootstrap.yml`中的配置拷贝过来就完事了, 并且`enabled: false`配置上去, 我们在课程中学过了, 这个配置是用来关闭`nacos`的配置中心的, 所以打印出来的变量都是本地的变量了, 这方便我们来学习切换环境

```yml
spring:  
  application:  
    # 配置服务的名称  
    name: gulimall-coupon  
  cloud:  
    nacos:  
      discovery:  
        # 配置nacos服务端地址  
        server-addr: 127.0.0.1:8848  
      config:  
        # 配置中心服务器地址  
        server-addr: 127.0.0.1:8848  
        # 配置文件扩展名  
        file-extension: yaml
        enabled: false
```

- bootstrap-prod.yml

同上, 因为我没有生产环境服务器的地址

- application.yml

直接删除, 没用了

application-dev.yml

就是把`application.yml`中的配置拷贝过来了

```yml
server:  
  # 服务端口号  
  port: 7001  
  servlet:  
    encoding:  
      # 返回数据使用utf-8编码  
      charset: utf-8  
      # 强制使用utf-8, 否则某些浏览器中查看会乱码  
      force: true  
  
spring:  
  datasource:  
    # 数据库连接url  
    url: jdbc:mysql://localhost:3306/gulimall_sms  
    # 驱动类名  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    # 数据库用户名  
    username: root  
    # 数据库密码  
    password: 123456  
  
mybatis-plus:  
  # 配置mapper存放位置  
  mapper-locations: classpath:mapper/**/*.xml  
  # 配置日志打印策略  
  configuration:  
    log-impl: org.apache.ibatis.logging.nologging.NoLoggingImpl  
  # 配置主键生成策略  
  global-config:  
    db-config:  
      id-type: auto  
  
coupon:  
  user:  
    name: "张三"  
    age: 18
```

- application-prod.yml

配置变一下方便测试, 其他同上

```yml
coupon:  
  user:  
    name: "王五"  
    age: 30
```

## 🌲 测试

创建完目录就是这个样子

![](images/Pasted%20image%2020231001174821.png)

配置好了我们重启服务试试吧, 在启动过程中可以看到这句话

```
The following 1 profile is active: "dev"
```

说明读取了带dev后缀的配置文件

http://localhost:7001/coupon/coupon/test

```json
{"msg":"success","code":0,"data":"张三 18"}
```

然后我们在`bootstrap.yml`中把环境改成`prod`, 然后重启

```yml
spring:  
  profiles:  
    active: prod
```

同样看到了这句话

```
The following 1 profile is active: "prod"
```

然后我们访问试试

http://localhost:7001/coupon/coupon/test

```
{"msg":"success","code":0,"data":"王五 30"}
```

因为我们在生产的配置文件中写的名字是`王五`, 所以被读出来了

## 🌲 配置区分

在学习之前我们先把环境调成`dev`, 然后读取配置文件的开关打开`enabled: false`

做完「上面这句话」的操作后, 我们再来继续学习

学会了`springboot`层面的切换环境, 那我们的配置中心也是一样的, 我们新建配置文件`gulimall-coupon-dev.yaml`, 聪明的你想必知道为什么这么起名字, 因为配置nacos可以根据你的环境来读取相应配置

![](images/Pasted%20image%2020231001181156.png)

然后我们重启程序, 访问接口

http://localhost:7001/coupon/coupon/test

```json
{"msg":"success","code":0,"data":"dev环境测试 18"}
```

我们发现`nacos`也能够通过`spring.profiles.active`的值来读取配置文件, 那么生产环境的我们也可以做测试了, 新建`gulimall-coupon-prod.yaml`

```yml
coupon:
  user:
    name: "prod环境测试"
      age: 18
```

然后我们把环境切换到生产

```yml
spring:  
  profiles:  
    active: prod
```

然后我们重启应用

http://localhost:7001/coupon/coupon/test

```json
{"msg":"success","code":0,"data":"prod环境测试 18"}
```

我们发现也是可以读到生产环境配置的

## 🌲 总结

我们发现通过`spring.profiles.active`属性既可以切换`springboot环境`又可以切换`分布式配置中心nacos`, 简直是一举两得, 比`命名空间`和`分组`方便的不是一点半点, 如果是普通的项目, 那我可以宣布, 这就叫够用! 其他两个你都不要学习了, 用到之后在学

# 🍎 命名空间区分

学了上面的`dataId`区分方式已经满足了大部分的需求, 然后我们继续来看视频中讲的根据命名空间区分, 视频里主要说了在命名空间中的两种区分方式

> 根据环境区分

把命名空间起名为`dev prod test`, 先说结论, 我觉得不太好, 切换环境我们使用`dataId`就是最简单的, 而且可以和`sping-boot`环境同步

> 根据服务区分

我觉得这个也不太好, 因为同样的也可以使用`dataId`区分

所以命名空间究竟是用来区分什么呢? 我查了很多资料上面说的我基本上也不能认同, 我询问了实际做Java项目的朋友, 他说nacos只是提供了不同的维度作为区分条件, 他们用group来区分环境, 因此我也在v2ex中进行了提问, 但是由于是十月一都在休息貌似没有人回答我, 所以这一块暂且放下先学用法吧

https://www.v2ex.com/t/978541#reply0

我们先来看命名空间, 可以在nacos中点击命名空间的分页进入到命名空间

![](images/Pasted%20image%2020231002085031.png)

点击新建就可以创建命名空间了

![](images/Pasted%20image%2020231002085051.png)

然后视频中的例子是用环境名作为命名空间的名字

![](images/Pasted%20image%2020231002102545.png)

然后分别创建出另外几个`prod 生产环境`, `test 测试环境`, 创建完是这个样子的

![](images/Pasted%20image%2020231002102738.png)

我们可以看到每个命名空间都会自动生成一个id, 我们只需要把这个id配置到项目中就能读取配置, 那么我们来配置一下吧

```yml
spring:  
  application:  
    # 配置服务的名称  
    name: gulimall-coupon  
  cloud:  
    nacos:  
      discovery:  
        # 配置nacos服务端地址  
        server-addr: 127.0.0.1:8848  
      config:  
        # 配置中心服务器地址  
        server-addr: 127.0.0.1:8848  
        # 配置文件扩展名  
        file-extension: yaml  
        # 开启配置中心 默认是true
        enabled: true  
        # 配置命名空间  
        namespace: 0bc5266b-8a91-430e-b9df-d864c9f0588b
```

我们可以看到把命名空间的id配置到项目中, 项目就会自动取读取这个命名空间下面的配置了, 注意dev和prod环境都要配置上这个`namespace: 0bc5266b-8a91-430e-b9df-d864c9f0588b`, 否则会有环境不生效

然后我们就可以把配置克隆到里面

![](images/Pasted%20image%2020231003125229.png)

克隆后我们把配置修改一下

```yml
coupon:
  user:
    name: "dev环境测试 命名空间dev"
      age: 18

coupon:
  user:
    name: "prod环境测试 命名空间dev"
      age: 18
```

然后就正常使用就可以了

http://localhost:7001/coupon/coupon/test

```json
{"msg":"success","code":0,"data":"dev环境测试 命名空间dev 18"}
{"msg":"success","code":0,"data":"prod环境测试 命名空间dev 18"}
```

然后视频里也讲了一个把服务名作为配置中心区分, 我这里就不说了一个道理, 但是我们发现这种配置id来区分服务的方式很麻烦, 至少比`dataId`自动区分配置是麻烦的, 增加了配置成本

# 🍎 分组区分

这个区分方式也比较简单, 我们在以前配置中都使用默认的组别, 现在我们使用点别的

![](images/Pasted%20image%2020231003130555.png)

把配置克隆一下就可以了, 换个组名叫`NEW_GROUP`

```yml
coupon:
  user:
    name: "dev环境测试 group:NEW_GROUP"
      age: 18

coupon:
  user:
    name: "prod环境测试 group:NEW_GROUP"
      age: 18
```

然后把组配置上

```
spring:  
  application:  
    # 配置服务的名称  
    name: gulimall-coupon  
  cloud:  
    nacos:  
      discovery:  
        # 配置nacos服务端地址  
        server-addr: 127.0.0.1:8848  
      config:  
        # 配置中心服务器地址  
        server-addr: 127.0.0.1:8848  
        # 配置文件扩展名  
        file-extension: yaml  
        # 开启配置中心  
        enabled: true  
        # 配置命名空间  
        #        namespace: 0bc5266b-8a91-430e-b9df-d864c9f0588b  
        # 配置组  
        group: NEW_GROUP
```

然后就正常使用就可以了

http://localhost:7001/coupon/coupon/test

```json
{"msg":"success","code":0,"data":"dev环境测试 group:NEW_GROUP 18"}
{"msg":"success","code":0,"data":"prod环境测试 group:NEW_GROUP 18"}
```

# 🍎 多配置

根据视频上面的思想, 如果配置多了放下一个配置中会比较臃肿, 不好维护, 所以我们可以选择把不同的配置拆分写到不同的文件中, 然后把这些文件组合起来变成一个大的配置, 我们现在就来试试吧

```yml
spring:  
  application:  
    # 配置服务的名称  
    name: gulimall-coupon  
  cloud:  
    nacos:  
      discovery:  
        # 配置nacos服务端地址  
        server-addr: 127.0.0.1:8848  
      config:  
        # 配置中心服务器地址  
        server-addr: 127.0.0.1:8848  
        # 配置文件扩展名  
        file-extension: yaml  
        # 扩展配置  
        ext-config:  
          - data-id: datasource.yml  
            group: EXT_GROUP  
            refresh: true  
          - data-id: mybatis.yml  
            group: EXT_GROUP   
            refresh: true  
          - data-id: other.yml  
            group: EXT_GROUP  
            refresh: true
```

我们和视频上配置是一样的, 为了方便就不写`-dev -prod`之类的了, 否则要创建6个文件, 话说回来, 如果配置中心读不到带`-环境`的配置文件就会去读`服务名.yaml`这没什么问题, 那我们就把配置放过去吧

- datasource.yml

```yml
spring:  
  datasource:  
    # 数据库连接url  
    url: jdbc:mysql://localhost:3306/gulimall_sms  
    # 驱动类名  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    # 数据库用户名  
    username: root  
    # 数据库密码  
    password: 123456
```

- mybatis.yaml

```yml
mybatis-plus:  
  # 配置mapper存放位置  
  mapper-locations: classpath:mapper/**/*.xml  
  # 配置日志打印策略  
  configuration:  
    log-impl: org.apache.ibatis.logging.nologging.NoLoggingImpl  
  # 配置主键生成策略  
  global-config:  
    db-config:  
      id-type: auto
```

- other.yaml

```yml
server:  
  # 服务端口号  
  port: 7001  
  servlet:  
    encoding:  
      # 返回数据使用utf-8编码  
      charset: utf-8  
      # 强制使用utf-8, 否则某些浏览器中查看会乱码  
      force: true
```

然后我们把配置文件中的配置都注释掉后, 重启项目

如果发现项目启动成功说明读取成功了, 我们可以在下面的log中看到项目加载了哪些配置

```
[Nacos Config] Listening config: dataId=datasource.yaml, group=EXT_GROUP
[Nacos Config] Listening config: dataId=gulimall-coupon-dev.yaml, group=DEFAULT_GROUP
[Nacos Config] Listening config: dataId=mybatis.yaml, group=EXT_GROUP
[Nacos Config] Listening config: dataId=other.yaml, group=EXT_GROUP
[Nacos Config] Listening config: dataId=gulimall-coupon.yaml, group=DEFAULT_GROUP
[Nacos Config] Listening config: dataId=gulimall-coupon, group=DEFAULT_GROUP
```

到此为止, 先说看法我并不会使用这种配置, 因为我觉得文件越多的情况下越不方便管理, 我还是会坚持使用`服务名-环境.yaml`来配置































































































