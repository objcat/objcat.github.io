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

![](./images/ff12ece0835f31fc48290d971b75cb44/1.png)

![](./images/ff12ece0835f31fc48290d971b75cb44/2.png)

![](./images/ff12ece0835f31fc48290d971b75cb44/3.png)

2.配置pom文件 (maven配置文件)

![](./images/ff12ece0835f31fc48290d971b75cb44/4.png)


![](./images/ff12ece0835f31fc48290d971b75cb44/5.png)

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

![](./images/ff12ece0835f31fc48290d971b75cb44/6.png)

好 但是我们又会发现项目报错了 不要慌 我们需要再次更新一个maven

![](./images/ff12ece0835f31fc48290d971b75cb44/7.png)

更新完之后我们发现红叉没了

![](./images/ff12ece0835f31fc48290d971b75cb44/8.png)

到这里为止环境已经搭建完成 如果有报错请自行百度解决

###三.开始写接口

首先我们需要创建两个包

![](./images/ff12ece0835f31fc48290d971b75cb44/9.png)

在目录上点右键创建第一个包
![](./images/ff12ece0835f31fc48290d971b75cb44/10.png)

创建第二个包
![](./images/ff12ece0835f31fc48290d971b75cb44/11.png)

到这里两个包都创建好了 不会的自己去练习一下 不懂的 百度揣摩一下

好 下面我们开始创建类  这些类就是用来配置程序入口和写接口的

![](./images/ff12ece0835f31fc48290d971b75cb44/12.png)


填写类名  我这里用AppDelegate作为类名  iOS开发应该一眼就能看出来这是干什么用的 没错 就是程序入口

![](./images/ff12ece0835f31fc48290d971b75cb44/13.png)


好了finish

然后在类中写入下面的代码
![](./images/ff12ece0835f31fc48290d971b75cb44/14.png)

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

![](./images/ff12ece0835f31fc48290d971b75cb44/15.png)

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

![](./images/ff12ece0835f31fc48290d971b75cb44/16.png)


我们这时会看到控制台在跑
![](./images/ff12ece0835f31fc48290d971b75cb44/17.png)


这些信息有些是对我们有用的 比如 我们可以看到自己服务器的端口号

![](./images/ff12ece0835f31fc48290d971b75cb44/18.png)


好了 看到上面的画面 我们本地服务器就已经启动成功了

好了我们来测试一下自己写的接口吧


![](./images/ff12ece0835f31fc48290d971b75cb44/19.png)

我们用浏览器跑一下

![](./images/ff12ece0835f31fc48290d971b75cb44/20.png)

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
![](./images/ff12ece0835f31fc48290d971b75cb44/21.png)

这次返回的是一个json  我们在web请求一下试试
![](./images/ff12ece0835f31fc48290d971b75cb44/22.png)


我们需要把localhost换成本机的内网ip 所以ipconfig一下
![](./images/ff12ece0835f31fc48290d971b75cb44/23.png)

好 我们知道了地址为
```
http://192.168.1.110:8080/third
```

我们在iOS中请求一下

![](./images/ff12ece0835f31fc48290d971b75cb44/24.png)

返回结果是正确的

![](./images/ff12ece0835f31fc48290d971b75cb44/25.png)

#*****IDEA篇*****

###一.准备工具
(1).java
http://www.oracle.com/technetwork/java/javase/downloads/index.html
(2).idea开发工具
https://www.jetbrains.com/idea/  

###二.创建一个maven工程并引入jar包
![](./images/ff12ece0835f31fc48290d971b75cb44/26.png)

创建一个新项目
![](./images/ff12ece0835f31fc48290d971b75cb44/27.png)

![](./images/ff12ece0835f31fc48290d971b75cb44/28.png)

填写完 next -> finish 项目创建完毕

然后在工程中找到POM文件 我们开始熟练地导入依赖包

![](./images/ff12ece0835f31fc48290d971b75cb44/29.png)

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

![](./images/ff12ece0835f31fc48290d971b75cb44/30.png)


然后我们开始在java目录下创建包

![](./images/ff12ece0835f31fc48290d971b75cb44/31.png)

![](./images/ff12ece0835f31fc48290d971b75cb44/32.png)

然后点OK

![](./images/ff12ece0835f31fc48290d971b75cb44/33.png)

我们可以看到包已经创建好了 

接下来我们设置一下IDE工程目录的浏览属性把文件夹结构分开(方便创建文件)
![](./images/ff12ece0835f31fc48290d971b75cb44/34.png)

点击小齿轮  然后把Hide Empty Middle Packages这个勾勾干掉

这是我们就可以看到 文件夹分开了
![](./images/ff12ece0835f31fc48290d971b75cb44/35.png)


我们选择objcat这个文件夹创建文件AppDelegate类
![](./images/ff12ece0835f31fc48290d971b75cb44/36.png)

点击OK创建完毕
![](./images/ff12ece0835f31fc48290d971b75cb44/37.png)

创建之后是这个样子
![](./images/ff12ece0835f31fc48290d971b75cb44/38.png)

然后我们开始写入程序入口的代码


![](./images/ff12ece0835f31fc48290d971b75cb44/39.png)

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

![](./images/ff12ece0835f31fc48290d971b75cb44/40.png)

> 创建之后是这个样子  我们可以清晰的看到 TestController和AppDelegate不是在同一级目录下的 目录千万不要搞错了!!!!

好我们继续写入TestController的代码

![](./images/ff12ece0835f31fc48290d971b75cb44/41.png)

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
![](./images/ff12ece0835f31fc48290d971b75cb44/42.png)

我们可以看到程序跑起来了

![](./images/ff12ece0835f31fc48290d971b75cb44/43.png)

下面可以看到启动的时间和端口号
![](./images/ff12ece0835f31fc48290d971b75cb44/44.png)


下面我们来访问一下
http://localhost:8080/hello

![](./images/ff12ece0835f31fc48290d971b75cb44/45.png)

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

![](./images/ff12ece0835f31fc48290d971b75cb44/46.png)

然后我们测试一下接口

![](./images/ff12ece0835f31fc48290d971b75cb44/47.png)


是可以访问成功的.




























#finally enjoy it.
#write by objcat 2018.02.04
#update by objcat 2018.03.29
