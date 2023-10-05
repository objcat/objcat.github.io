# 🍎 前言

本文是跟随尚硅谷「谷粒商城」视频去做的一个项目, 也是白猫人生中真正意义上的第一个`Java`项目

https://www.bilibili.com/video/BV1np4y1C7Yf

# 🍎 我的仓库

https://gitee.com/objcat/gulimall2024

# 🍎 学习日志

下面是白猫的学习日志, 顺便透漏一下, 白猫从`2018`年开始就在学习`spring-cloud`相关的项目了, 当时用的还是全家桶`eureka, histrix, zuul`但仅仅是学习, 并没有实打实的做什么项目, 中途也放弃过一段直到最近`2`年内才开始逐渐搞技术更新, 学习了`nacos, sentinel, shiro, security, gateway`等, 所以下面所用的时间是在白猫已经有了「少量技术积累」的前提下进行的, 所以学习的速度可能会快一些, 新手的话慢慢看, 不要求快速掌握技术, 只要一步一个脚印应该都可以学会, 这也是我写这篇文档的初衷, 我觉得或许可以在一定程度上解答某些人的困惑吧, 为此白猫也建立了「白猫Java交流群 」群号:`916867721`, 用于讨论开发中遇到的问题, 无培训机构, 无广告, 创建日期 `2023.09.29 18:00`

| 日期 | 学习内容 |
| -- | -- |
| 2023.09.25 | 微服务框架(父工程+子工程+数据库), 后台管理系统后端 |
| 2023.09.26 | 后台管理系统前端, 代码自动生成 |
| 2023.09.27 | nacos注册中心 |
| 2023.09.28 | openfeign |
| 2023.09.29 | nacos配置中心 |
| 2023.10.01 | nacos配置中心 |
| 2023.10.02 | nacos配置中心 |
| 2023.10.03 | nacos配置中心, gateway网关 |
| 2023.10.04 | 三级分类 |

# 🍎 架构图

看着非常唬人那么我们就跳过这段直接去学习搭建项目吧

![](images2/Pasted%20image%2020230925002042.jpg)

# 🍎 开始

## 🌲 环境搭建

按照视频中的要求去安装就行, 我顺便提供一下我的安装方式, 仅供参考

- docker [跳转docker_env](../../../3-program/env/docker/docker_env.md)
- java [跳转 java_env](../../../3-program/env/java/java_env.md)
- mysql [跳转 mysql_env](../../../3-program/env/mysql/mysql_env.md)
- redis [跳转 redis_env](../../../3-program/env/redis/redis_env.md)
- node [跳转 node_env](../../../3-program/env/node/node_env.md)

那我也提供一下我的环境, 因为视频中环境比较老, 以至于我无法忍受, 所以我把他们能换的都换成新的环境了

```shell
docker -v
Docker version 24.0.6, build ed223bc

java -version
openjdk version "17.0.5" 2022-10-18

node -v
v18.16.1
npm -v
9.5.1

mysql:8.0.27

redis:7
```

## 🌲 微服务框架

然后根据视频来创建项目

### 🌸 创建父工程

`jdk`最好用`1.8`因为是目前的「主流版本」😂  我这里是逆天而行选的`java17`, 但要注意的是你选择`java17`未来在服务器运行的时候就要使用`java17`的`jdk`否则会跑不起来

![](images2/Pasted%20image%2020230925150719.png)

他是后配置父模块, 我这里是先配置父模块的`pom`, 我选择的版本是`2.7.15`

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.objcat</groupId>
    <artifactId>gulimall2024</artifactId>
    <version>1.0</version>
    <packaging>pom</packaging>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.plugin.version>3.8.1</maven.compiler.plugin.version>
        <lombok.version>1.18.22</lombok.version>
        <mybatis-plus.version>3.5.3.1</mybatis-plus.version>
        <spring-boot.version>2.7.15</spring-boot.version>
        <spring-cloud.version>2021.0.6</spring-cloud.version>
        <spring-cloud-alibaba.version>2021.0.5.0</spring-cloud-alibaba.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring-cloud-alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>${mybatis-plus.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${spring-boot.version}</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>${maven.compiler.plugin.version}</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

	<repositories>
		<repository>
			<id>aliyun</id>
			<name>aliyun</name>
			<url>https://maven.aliyun.com/repository/public</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
	</repositories>
	
	<pluginRepositories>  
	    <pluginRepository>  
	        <id>aliyun-plugin</id>  
	        <url>https://maven.aliyun.com/repository/public</url>  
	        <releases>  
	            <enabled>true</enabled>  
	        </releases>  
	        <snapshots>  
	            <enabled>false</enabled>  
	        </snapshots>  
	    </pluginRepository>  
	</pluginRepositories>

</project>
```

### 🌸 创建微服务

视频中要求创建以下服务

- 商品服务 gulimall-product
- 订单服务 gulimall-order
- 仓储服务 gulimall-ware
- 优惠券服务 gulimall-coupon
- 用户服务 gulimall-member

一个一个去创建, 并且依赖只配置`web和openfeign`

![](images2/Pasted%20image%2020230925153931.png)

我这边不喜欢使用`Spring Initializr`, 就使用了IDEA自带的`new Module`, 把微服务一个一个的创建出来, 这里就贴一张图, 不演示了, 然后你可以把一个一个把配置粘到微服务中去, 我嫌那样比较麻烦, 所以我是创建了通用模块, 然后让子项目依赖于通用模块

### 🌸 创建通用模块

我创建了一个通用模块

![](images2/Pasted%20image%2020230925154647.png)

然后配置一下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.objcat</groupId>
        <artifactId>gulimall2024</artifactId>
        <version>1.0</version>
    </parent>

    <artifactId>gulimall-api-common</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
    </dependencies>

</project>
```

### 🌸 配置微服务pom

然后把每个微服务都配上这个依赖包, 我就用`product`来举例子了

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.objcat</groupId>
        <artifactId>gulimall2024</artifactId>
        <version>1.0</version>
    </parent>

    <artifactId>gulimall-product</artifactId>

    <dependencies>
        <dependency>
            <groupId>com.objcat</groupId>
            <artifactId>gulimall-api-common</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>

</project>
```

这样我们的`product`就继承了`common`中的依赖了, 创建之后是这样的

![](images2/Pasted%20image%2020230925155615.png)

## 🌲 上传到码云

然后说是要上传到码云, 那我就开一个仓库

![](images2/Pasted%20image%2020230925160333.png)

然后把仓库拉取下来, 代码复制到里面去, 推送

![](images2/Pasted%20image%2020230925160853.png)

推送完毕后我们可以看到, 文件都上去了

![](images2/Pasted%20image%2020230925161035.png)

## 🌲 配置数据库

视频中是使用`power designer`来设计的数据库, 但是`mac`上面没有, 我先加群拿资料, 但是管理员反应的很慢, 所以我在下面的网盘中拿的

```
所有文档资料，老乡点个赞再走！
链接：https://pan.baidu.com/s/18FuF760AYt3kILGWCmXVEA 
提取码：yyds
```

后来发现管理员没让我进群

![](images2/Pasted%20image%2020230925170951.png)

不进就不进有资料就行, 我们下载的资料一眼就能看到db

![](images2/Pasted%20image%2020230925172010.png)

因为每一个微服务都有自己的数据库, 每一个数据库中都有一张表, 所以我们把这些表都导入到数据库, 数据库前面已经安装了, 这里就直接导入表

![](images2/Pasted%20image%2020230925171918.png)

先把db拷贝到工程中方便导入, 然后我们直接使用IDEA自带的`dataGrip`进行导入, 先连接数据库

![](images2/Pasted%20image%2020230925172436.png)

连接完毕后创建这几个数据库

![](images2/Pasted%20image%2020230925172857.png)

- gulimall_admin 后台管理系统
- gulimall_oms 订单系统
- gulimall_pms 商品系统
- gulimall_sms 营销系统
- gulimall_ums 用户系统
- gulimall_wms 库存系统

创建可以点击数据库然后新建`schema`, 但这种模式创建起来很慢

![](images2/Pasted%20image%2020230925173043.png)

所以我们直接执行SQL

```sql
CREATE SCHEMA IF NOT EXISTS gulimall_admin;  
CREATE SCHEMA IF NOT EXISTS gulimall_oms;  
CREATE SCHEMA IF NOT EXISTS gulimall_pms;  
CREATE SCHEMA IF NOT EXISTS gulimall_sms;  
CREATE SCHEMA IF NOT EXISTS gulimall_ums;  
CREATE SCHEMA IF NOT EXISTS gulimall_wms;
```

然后对号入座, 开始导入

![](images2/Pasted%20image%2020230925172403.png)

然后选一个数据库导入进去

![](images2/Pasted%20image%2020230925173522.png)

导入完毕后是这个样子的

![](images2/Pasted%20image%2020230925173720.png)

可以看到每个里面都有很多张表

## 🌲 后台管理系统

视频上强烈推荐人人开源, 上面的是后台管理系统的服务端, 下面是后台管理系统的前端页面

https://gitee.com/renrenio

### 🌸 后台

https://gitee.com/renrenio/renren-fast

直接下载就行

![](images2/Pasted%20image%2020230925174440.png)

根据视频的操作, 直接把项目放到工程中, 这个db不用导入, 因为上面的`gulimall_admin`就是

![](images2/Pasted%20image%2020230925175715.png)

放进来之后没有识别, 不要慌我们给它配置进来, 点击`import maven`

![](images2/Pasted%20image%2020230925175641.png)

然后会发现一大堆的错误

![](images2/Pasted%20image%2020230925180305.png)

```
'parent.relativePath' of POM io.renren:renren-fast:3.0.0 (/Users/objcat/project/Java/gulimall2024/renren-fast/pom.xml) points at com.objcat:gulimall2024 instead of org.springframework.boot:spring-boot-starter-parent, please verify your project structure
```

我们一个一个解决, 首先把`pom`加上`<relativePath/>`, 因为我们把一个`springboot`项目拖拽到了`springcloud`的父项目中, 而它的父项目并不是我们的`gulimall2024`所以报错了

```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.6.6</version>
	<relativePath/>
</parent>
```

解决完问题后我们配置一下数据库, 把它运行起来, 我们找到`application-dev.yml`开发环境的配置文件, 修改数据库链接, 以及用户名和密码

```java
url: jdbc:mysql://localhost:3306/gulimall_admin?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai
username: root
password: 123456
```

我们找到启动文件把它运行起来

![](images2/Pasted%20image%2020230925181649.png)

然后我看到了一个报错, 在`shiro`的数据源中

```
/Users/objcat/project/Java/gulimall2024/renren-fast/src/main/java/io/renren/modules/sys/jwt/JWTRealm.java:45:27
java: 找不到符号
  符号:   方法 getUserId()
  位置: 类型为io.renren.modules.sys.entity.SysUserEntity的变量 user
```

看了很久不知道怎么回事, 后来更新了一下`lombok`好了, 我把java版本也改成了17

```
// <lombok.version>1.18.4</lombok.version>
<lombok.version>1.18.22</lombok.version>
```

### 🌸 后台管理系统

https://gitee.com/renrenio/renren-fast-vue

前台我们也放进项目里, 为了方便管理, 并且使用`vscode`来打开项目

```
cd renren-fast-vue 
ls

CHANGELOG.md            README.md               config                  gulpfile.js             package-lock.json       src                     test
LICENSE                 build                   demo-screenshot         index.html              package.json            static
```

可以看到里面有一个`package.json`首先我们就要安装依赖

```
yarn
```

你也可以使用`npm`如果不会就过去学 [跳转 npm](../../../4-package-manager/npm/npm.md)

安装过程中我们发现`node-sass`报错了, 说没有xxx命令不能编译, 网上的解决方案是什么下载c++的编译环境, 但是我没有采用, 我是把它的版本进行了更新

```shell
yarn add node-sass@latest

"node-sass": "^9.0.0"
```

然后使用`yarn run`就可以运行了, 但这只是侥幸

后来我换了另外一台m2的机器, 出现了下面的问题

```
[4/7] ⠠ node-sass
error /Users/objcat/project/Java/gulimall2024/renren-fast-vue/node_modules/chromedriver: Command failed.
```

可见`node-sass`确实是个坎, 遇到问题解决问题, 我们慢慢来研究, 首先我们来看node-sass匹配的版本

https://www.npmjs.com/package/node-sass/v/8.0.0

![](images/Pasted%20image%2020231005123751.png)

因为我是node18, 所以我需要使用`8.0+`的版本, 那么可以印证`9.0`没问题

```
node -v                         

v18.17.1
```

于是我尝试了

```
yarn add chromedriver
```

发现编译通过了

![](images/Pasted%20image%2020230926112559.png)

运行出来界面是这样的, 那我们前端也跑起来了, 然后我们可以登录进去看看

```
用户名:admin
密码:admin
```

![](images/Pasted%20image%2020230926121632.png)

然后就可以看到主页了

![](images/Pasted%20image%2020230926121720.png)

## 🌲 解决报错 ⭐ (难)

https://gitee.com/renrenio/renren-generator

然后根据视频步骤, 他让我们下载一个代码生成器, 

### 🌸 配置代码生成器

然后还是复制到工程后`import`, 随后改一下`pom`, 跟管理系统后台一样, 添上

```
<relativePath />
```

注意视频里的方法是让你把「生成器工程」当做一个`module`配置到父工程中, 我是不推荐这种做法的, `屎尿搅一块了`, 看着烦, 所以我都是采用`<relativePath />`作为单独工程来运行, 我们可以看到`maven`结构是这样的

![](images/Pasted%20image%2020230926134323.png)

然后就可以让`maven`拉取依赖了, 拉取完依赖我们要配置一下项目, 先配置一下数据库, 因为是自动生成代码的工程, 而代码要根据表中的结构来, 所以要生成哪个库的表就要配置那个库, 我这里用`gulimall_pms`作为例子, 先生成这个库的代码

```yml
url: jdbc:mysql://localhost:3306/gulimall_pms?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai
username: root
password: 123456
```

配置完我们再配置生成策略`generator.properties`, 因为我的名字叫`objcat`, 所以你们可以坚持你们的`atguigu`或是其他名字都可以

```properties
# 主目录
mainPath=com.objcat
# 包名
package=com.objcat
moduleName=product
# 作者
author=objcat
# Email
email=objcat@gmail.com
# 表前缀(类名不会包含表前缀)
tablePrefix=pms_
```

配置过程中看到中文字可能会出现乱码, 所以我们要修改一处配置

![](images/Pasted%20image%2020230926124617.png)

然后我们就可以启动了, 启动之前给它的主启动类改了个名

![](images/Pasted%20image%2020230926124920.png)

然后端口改了个`9002`

```
server:
  port: 9002
```

然后我们启动, 启动后就可以访问了

http://localhost:9002

然后我们点击红框

![](images/Pasted%20image%2020230926125421.png)

可以看到表都出来了, 根据视频要求把所有表全全选, 但是记住一点要勾掉`undo_log`表, 否则后面处理起来会比较麻烦, 我们先选择每页200个然后全选否则点不全, 然后点生成代码

![](images/Pasted%20image%2020230926180624.png)

然后可以看到下载了一个压缩包

![](images/Pasted%20image%2020230926125634.png)

然后打开压缩包就能看见一个main文件夹

![](images/Pasted%20image%2020230926125822.png)

视频中要求直接把这个main粘贴到项目中去, 我们删除原来的`main`, 然后把生成的`main`粘贴进去, 粘贴完成后是这样的

![](images/Pasted%20image%2020230926130733.png)

### 🌸 查看报错

我们可以看到生成的代码结构还是比较好的, 是我们的`三层架构`, 然后我们随便打开一个控制器或者`service`, 发现报红了

![](images/Pasted%20image%2020230926133844.png)

打开`service`也同样是报红

![](images/Pasted%20image%2020230926133934.png)

### 🌸 配置mybatis-plus

我们现在就要来解决这些依赖库和工具缺失的问题, 因为我之前已经创建过`gulimall-api-common`了, 所以直接在里面添加, 首先添加`mybatis-plus`, 注意两个依赖必须都添加

```xml
<dependency>
	<groupId>com.baomidou</groupId>
	<artifactId>mybatis-plus-boot-starter</artifactId>
</dependency>

<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
</dependency>
```

### 🌸 配置lombok

然后视频让配`lombok`, 就是一个让你省代码的工具, 自动生成`getter/setter`方法, 如果你按照我的配置文件来是不需要配这个的, 我的父`pom`里面已经有了

### 🌸 配置utils

我们来看一下需要的工具类的路径

```
import com.objcat.common.utils.PageUtils;
```

可以看到公共类的路径是这样的, 那我们就创建出这样的目录, 然后把`renren-fast`项目中的`common`包全完复制过来, 拷贝过来后目录结构是这样的

![](images/Pasted%20image%2020230926135647.png)

然后我们发现utils里面有个R报错了, 我们需要导入依赖库

```xml
<dependency>
	<groupId>org.apache.httpcomponents</groupId>
	<artifactId>httpcore</artifactId>
	<version>4.4.16</version>
</dependency>

<dependency>
	<groupId>commons-lang</groupId>
	<artifactId>commons-lang</artifactId>
	<version>2.6</version>
</dependency>

<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

然后根据视频要求删掉`XssFilter.java`和`XssHttpServletRequestWrapper.java`, 然后其他的自己根据视频来做

### 🌸 修改模板

然后我们会发现还是有一个

```
@RequiresPermissions("product:categorybrandrelation:list")
```

在报错, 我们可以修改自动生成代码的模板, 比手动一个一个改来的快, 很简单啊, 看图

![](images/Pasted%20image%2020230926141100.png)

然后都注释掉就可以了, 然后重启项目再重新生成一套代码, 注释掉的地方大概两大类

```
## import org.apache.shiro.authz.annotation.RequiresPermissions;

## @RequiresPermissions("${moduleName}:${pathName}:list")
......
```

然后我发现我的项目不在报错了, 如果你的还报错, 就继续改, 这个地方对新手是「毁灭性打击」, 如果你能根据视频配出来, 那么我可以说你是半个天才, 所以我推荐你直接拷贝我的`common`使用, 先把功能做出来

## 🌲 创建服务

下面教学如何启动服务, 我们需要有配置文件和主启动类

### 🌸 启动服务

我们新建`application.yml`

```yml
server:
  # 服务端口号
  port: 8081
  servlet:
    encoding:
      # 返回数据使用utf-8编码
      charset: utf-8
      # 强制使用utf-8, 否则某些浏览器中查看会乱码
      force: true

spring:
  datasource:
    # 数据库连接url
    url: jdbc:mysql://localhost:3306/gulimall_pms
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

然后写一个启动文件, 注意我们这里没有使用`MapperScan`因为我看生成代码的地方都有`@Mapper`注解

```java
@SpringBootApplication
public class ProductApplication {
    public static void main(String[] args) {
        SpringApplication.run(ProductApplication.class, args);
    }
}
```

然后我们运行一下项目, 发现报错了

![](images/Pasted%20image%2020230926173840.png)

这个表是`undo_log`, 里面发现有一个类型是`Longblob`, 我可以把它替换成了`Blob`, 但这个类不属于我们的系统, 可能是mysql自动创建的, 我们需要把它完全移除, 否则会报错, 如果你在前面没有勾选, 那么就谢天谢地了

然后我们运行项目, 能运行起来就对了

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::               (v2.7.15)
```

### 🌸 测试

按照视频要求它让我们写一个测试类去测试, 那来吧

![](images/Pasted%20image%2020230926181148.png)

注意目录比如与原工程保持一致, 我们`@Resource`代替`@Autowired`

```java
@SpringBootTest
public class ProductApplicationTests {

    @Resource
    BrandService brandService;

    @Test
    void contextLoads() {
        List<BrandEntity> list = brandService.list();
        System.out.println(list);
        // [BrandEntity(brandId=9, name=华为, logo=https://gulimall-hello.oss-cn-beijing.aliyuncs.com/2019-11-18/de2426bd-a689-41d0-865a-d45d1afa7cde_huawei.png, descript=华为, showStatus=1, firstLetter=H, sort=1), BrandEntity(brandId=10, name=小米, logo=https://gulimall-hello.oss-cn-beijing.aliyuncs.com/2019-11-18/1f9e6968-cf92-462e-869a-4c2331a4113f_xiaomi.png, descript=小米, showStatus=1, firstLetter=M, sort=1), BrandEntity(brandId=11, name=oppo, logo=https://gulimall-hello.oss-cn-beijing.aliyuncs.com/2019-11-18/5c8303f2-8b0c-4a5b-89a6-86513133d758_oppo.png, descript=oppo, showStatus=1, firstLetter=O, sort=1), BrandEntity(brandId=12, name=Apple, logo=https://gulimall-hello.oss-cn-beijing.aliyuncs.com/2019-11-18/819bb0b1-3ed8-4072-8304-78811a289781_apple.png, descript=苹果, showStatus=1, firstLetter=A, sort=1)]
    }
}
```

如果列表被打印出来证明是对的

## 🌲 完善其他服务

根据视频步骤来, 他让把其他服务也跟`product`一样使用自动生成代码配置上, 你可以去配置, 但是因为这是个体力活, 我选择先往后面看看, 有什么其他要改的, 需要用到的时候再去创建

## 🌲 Nacos

下面是视频中有关于nacos的章节 20 ~ 25

[跳转 gulimall_nacos](../gulimall_nacos/gulimall_nacos.md)

## 🌲 Gateway

下面是视频中有关于gateway的章节 26 ~ 27

[跳转 gulimall_gateway](../gulimall_gateway/gulimall_gateway.md)

## 🌲 前端基础

作为一个移动端来说, 终于是跳到和自己工作沾边的领域了, 不过前端和后端一起学习对于新手来说简直是「泰难啦」但是你心中要有一个理想就是学习完你就是全栈了

### 🌸 js语法

视频里主要讲的是es6的语法, 我这里直接跳过了, 你们看视频吧, 如果想学习, 白猫整理出了一份自己写的语法文档, 自己敲一敲即可

[跳转 javascript](../../../0-language/javascript/javascript.md)

如果不想学习`js`也无所谓, 但你至少要有一点语言基础, 比如会java, 然后可以在后续课程参照着写, 用到的时候直接去查

### 🌸 vue语法

这个也不想说了, 就是个模板引擎, 用于前后端分离的项目中, 如果你想学习那么白猫也有文档可以帮助你学习, 敲敲即可

[跳转 vue](../../../1-framework/javascript/vue/vue/vue.md)

## 🌲 product模块

[跳转 gulimall_product](../gulimall_product/gulimall_product.md)

