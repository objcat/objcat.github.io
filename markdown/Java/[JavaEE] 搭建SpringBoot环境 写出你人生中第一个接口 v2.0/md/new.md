最近一直在研究SpringBoot, 那么接下来我就教大家如何来搭建一个SpringBoot并写出来一个接口.
这里分成了Eclipse和IDEA篇  可以使用这两个IDE来进行环境的搭建. 使用IDEA的同学直接在文章中搜索`IDEA篇`

#*****Eclipse篇*****

####一.安装java环境和eclipse
(1).java
http://www.oracle.com/technetwork/java/javase/downloads/index.html
(2).eclipse  
https://www.eclipse.org/downloads/
自行安装 
####****在看第二步之前请确保你的java环境和eclipse环境已经安装完毕, 否则后果自负 = = .***

###二.创建一个maven工程并引入jar包
> 问:什么是maven? 答: 是第三方库管理工具

>问:什么是jar包 答: 是第三方库

好了我相信经过上面两个回答 已经解答了不少疑惑 之后我们来一步一步操作

1.创建maven工程

![创建maven工程](http://upload-images.jianshu.io/upload_images/2194379-7ea4826efa62a388.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

![勾选创建一个简单的项目模板.png](http://upload-images.jianshu.io/upload_images/2194379-fb5091a05acfc8a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![填写项目名.png](http://upload-images.jianshu.io/upload_images/2194379-00ebf38fd8b3f1ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.配置pom文件 (maven配置文件)

![打开pom文件](http://upload-images.jianshu.io/upload_images/2194379-c8043e467844954f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![配置pom文件](http://upload-images.jianshu.io/upload_images/2194379-3365cfed122e07cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
	<!-- 引入springboot-parent -->
	<parent>
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter -->
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
	</parent>

	<!-- 引入jar包 -->
	<dependencies>
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>
```

配置完之后 `ctrl + s` 保存 我们会发现eclipse会开始下载jar包,  请稍等片刻 ..... =  =


好下载完jar包之后我们会发现工程中多了一堆jar包

![maven管理的jar](http://upload-images.jianshu.io/upload_images/2194379-3054b00d88ceb43f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好 但是我们又会发现项目报错了 不要慌 我们需要再次更新一个maven

![更新maven](http://upload-images.jianshu.io/upload_images/2194379-3b8bca2b56200aa4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

更新完之后我们发现红叉没了

![更新后](http://upload-images.jianshu.io/upload_images/2194379-9f1d1cc51413266d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到这里为止环境已经搭建完成 如果有报错请自行百度解决

###三.开始写接口

首先我们需要创建两个包

![](../img/1.png)

在目录上点右键创建第一个包
![](../img/2.png)

创建第二个包
![](../img/3.png)

到这里两个包都创建好了 不会的自己去练习一下 不懂的 百度揣摩一下

好 下面我们开始创建类  这些类就是用来配置程序入口和写接口的

![](../img/4.png)


填写类名  我这里用AppDelegate作为类名  iOS开发应该一眼就能看出来这是干什么用的 没错 就是程序入口

![](../img/5.png)


好了finish

然后在类中写入下面的代码
![](../img/6.png)

```
package com.objcat;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AppDelegate {
	public static void main(String[] args) {
		SpringApplication.run(AppDelegate.class, args);
	}
}
```

设置完程序入口  我们来新建另一个类  这个类是用来写接口的 这个类你们自行创建

![](../img/7.png)

```
package com.objcat.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestController {
	
	@RequestMapping("/hello") //地址映射
	public String hello() {
		return "这是我的第一个接口";
	}
}

```

之后我们回到AppDelegate中运行一下代码

注意 一定要回到AppDelegate中才能运行成功!!

![](../img/8.png)


我们这时会看到控制台在跑
![控制台在跑](http://upload-images.jianshu.io/upload_images/2194379-d697d8b77539bdb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1024)


这些信息有些是对我们有用的 比如 我们可以看到自己服务器的端口号

![image.png](http://upload-images.jianshu.io/upload_images/2194379-fe0e9690b440dd28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


好了 看到上面的画面 我们本地服务器就已经启动成功了

好了我们来测试一下自己写的接口吧


![接口](http://upload-images.jianshu.io/upload_images/2194379-6992dd7c77cf3228.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们用浏览器跑一下

![接口测试成功](http://upload-images.jianshu.io/upload_images/2194379-4fb67b5b237baa31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

说到这里 你们应该懂得了 `@RequestMapping("/hello")` 的作用 没错 就是用来引导请求路径的


好 我们再来写一个接口 在TestController中重新写个方法

```
	@RequestMapping("/third")
	public Map<String, String> third() {
		Map<String, String> map = new HashMap<String, String>();
		map.put("name", "张三");
		map.put("age", "18");
		map.put("sex", "girl");
		return map;
	}
	
```

写完之后重新构建一下项目
![重新构建](http://upload-images.jianshu.io/upload_images/2194379-60be81575e5bc98c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这次返回的是一个json  我们在web请求一下试试
![image.png](http://upload-images.jianshu.io/upload_images/2194379-2c171a7d26df5697.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们需要把localhost换成本机的内网ip 所以ipconfig一下
![我的内网ip](http://upload-images.jianshu.io/upload_images/2194379-5b0121fcd114b636.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好 我们知道了地址为
```
http://192.168.1.110:8080/third
```

我们在iOS中请求一下

![屏幕快照 2018-02-04 下午1.59.26.png](http://upload-images.jianshu.io/upload_images/2194379-8349288ef224adb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

返回结果是正确的

![返回结果](http://upload-images.jianshu.io/upload_images/2194379-aa382fc765116d15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

#*****IDEA篇*****

###一.准备工具
(1).java
http://www.oracle.com/technetwork/java/javase/downloads/index.html
(2).idea开发工具
https://www.jetbrains.com/idea/  

###二.创建一个maven工程并引入jar包
![](../img/9.png)

创建一个新项目
![](../img/10.png)

![](../img/11.png)

填写完 next -> finish 项目创建完毕

然后在工程中找到POM文件 我们开始熟练地导入依赖包

![](../img/12.png)

```
	<!-- 引入springboot-parent -->
	<parent>
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter -->
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
	</parent>

	<!-- 引入jar包 -->
	<dependencies>
		<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>
```

导入之后 import Changes一下

导入成功后  我们会看到一大堆包

![](../img/13.png)


然后我们开始在java目录下创建包

![](../img/14.png)

![](../img/15.png)

然后点OK

![](../img/16.png)

我们可以看到包已经创建好了 

接下来我们设置一下IDE工程目录的浏览属性把文件夹结构分开(方便创建文件)
![](../img/17.png)

点击小齿轮  然后把Hide Empty Middle Packages这个勾勾干掉

这是我们就可以看到 文件夹分开了
![](../img/18.png)


我们选择objcat这个文件夹创建文件AppDelegate类
![](../img/19.png)

点击OK创建完毕
![](../img/20.png)

创建之后是这个样子
![](../img/21.png)

然后我们开始写入程序入口的代码


![](../img/22.png)

```
package com.objcat;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AppDelegate {
    public static void main(String[] args) {
        SpringApplication.run(AppDelegate.class, args);
    }
}
```

然后我们开始创建第二个类 TestController

![](../img/23.png)

> 创建之后是这个样子  我们可以清晰的看到 TestController和AppDelegate不是在同一级目录下的 目录千万不要搞错了!!!!

好我们继续写入TestController的代码

![](../img/24.png)

```
package com.objcat.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestController {

    @RequestMapping("/hello") //地址映射
    public String hello() {
        return "这是我的第一个接口";
    }
}
```

到这里我们切换到`AppDelegate`中运行程序
![](../img/25.png)

我们可以看到程序跑起来了

![](../img/26.png)

下面可以看到启动的时间和端口号
![](../img/27.png)


下面我们来访问一下
http://localhost:8080/hello

![](../img/28.png)

我们发现是可以访问成功的  到这里服务器搭建完毕

然后我们再来写个接口 在TestController中重新写个方法`third`

```
	@RequestMapping("/third")
	public Map<String, String> third() {
		Map<String, String> map = new HashMap<String, String>();
		map.put("name", "张三");
		map.put("age", "18");
		map.put("sex", "girl");
		return map;
	}
	
```

然后重新构建一下项目

![](../img/29.png)

然后我们测试一下接口

![](../img/30.png)


是可以访问成功的.




























#finally enjoy it.
#write by objcat 2018.02.04
#update by objcat 2018.03.29