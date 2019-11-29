
#一.安装tomcat

- step 1 下载tomcat服务器
无脑点开
http://tomcat.apache.org
![点击该下载按钮](http://upload-images.jianshu.io/upload_images/2194379-75eb83b0d8c5321b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)
![点击zip下载](http://upload-images.jianshu.io/upload_images/2194379-281da018f226587a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- step 2 安装
zip 解压出来是这么个东西
![解压后](http://upload-images.jianshu.io/upload_images/2194379-8cf11915b4c28e8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进入bin目录可以看到两个文件
![两个文件](http://upload-images.jianshu.io/upload_images/2194379-6eea8eb638cbb0d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这两个文件就是启动和关闭服务器用的

下面我们cd到bin目录下 来启动服务器
```
cd /Users/hfcb/Downloads/apache-tomcat-9.0.2/bin 
sudo sh startup.sh
```

会出现如下错误
![错误](http://upload-images.jianshu.io/upload_images/2194379-40570a63923365e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


因此我们需要给文件权限
```
chmod 777 *.sh
```

这里再次运行 
```
sudo sh startup.sh
```
我们发现服务器可以正常启动了 出现如下命令行
![正常启动](http://upload-images.jianshu.io/upload_images/2194379-d7a31159a2969c14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们在web访问以下试试吧 出现如下页面即为成功启动服务器
http://localhost:8080
![成功启动服务器](http://upload-images.jianshu.io/upload_images/2194379-68f40e6abd80fa2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)


接下来我们点击 Server Status
![Server Status](http://upload-images.jianshu.io/upload_images/2194379-40bea20e13c9b471.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

会出现输入用户名和密码  这里输入 
```
用户名: tomcat  
密码:tomcat
```
![输入](http://upload-images.jianshu.io/upload_images/2194379-1b4b65c683dc49e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击login

结果发现无法登陆  弹出一个空白的登陆窗口
![空白](http://upload-images.jianshu.io/upload_images/2194379-ea62ccfc557284e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这时我们点击取消会出现401画面

![image.png](http://upload-images.jianshu.io/upload_images/2194379-dd3c4973be2c61c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们复制红框中的文字 打开如图所示目录下的文件
![目录](http://upload-images.jianshu.io/upload_images/2194379-eb288fa66fe87033.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/2194379-0fc96f5d8cca6f06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们会看到如下页面 把文字复制到最下边 并修改用户名和密码都为 tomcat
![修改](http://upload-images.jianshu.io/upload_images/2194379-b869077d2ae246b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后我们重启服务器

先关闭
```
sudo sh shutdown.sh
```

然后重新启动
```
sudo sh startup.sh
```


然后再次登录
![再次登录](http://upload-images.jianshu.io/upload_images/2194379-0ce57be76226476c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

出现如下页面即为成功
![出现](http://upload-images.jianshu.io/upload_images/2194379-2eed5df2f8133cfd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)



#一.安装eclipse
无脑点开 下载
https://www.eclipse.org/downloads/
![下载](http://upload-images.jianshu.io/upload_images/2194379-62b6282fcaaf6080.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下载之后安装

在此之前请确保你已经安装过java环境  如果没安装去java下载一个jdk
http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html

#安装过程略....


安装之后打开eclipse  cmd + , (cmd + 逗号) 打开设置页面 点击runtime Environments

![设置](http://upload-images.jianshu.io/upload_images/2194379-5ec7651cd80b95c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/2194379-72fcf6845c572234.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/2194379-50ae3ef833845c6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择自己的服务器路径  finished
![image.png](http://upload-images.jianshu.io/upload_images/2194379-ab185f43eaf046fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/2194379-b392f001e046174d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


之后我们新建一个项目
![new](http://upload-images.jianshu.io/upload_images/2194379-d76b27e9a9c93d96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/2194379-960dec6251d14d12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击next

![image.png](http://upload-images.jianshu.io/upload_images/2194379-75a262840b605380.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


next

![image.png](http://upload-images.jianshu.io/upload_images/2194379-176b188eb4e3e2b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


next

钩一定要钩上

![image.png](http://upload-images.jianshu.io/upload_images/2194379-2948a0de4acf1267.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


之后我们新建一个jsp文件
![jsp](http://upload-images.jianshu.io/upload_images/2194379-d3714884ca8b4d5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/2194379-7cfb2337b6865793.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后我们会发现报一个错11

![exception](http://upload-images.jianshu.io/upload_images/2194379-29029041719bc87c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们需要去配置一下 Build Path

![step1](http://upload-images.jianshu.io/upload_images/2194379-4cd656e4c0a30413.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![step2](http://upload-images.jianshu.io/upload_images/2194379-a72b5c2d8e1b319d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![step3](http://upload-images.jianshu.io/upload_images/2194379-2a9a14e5a094c32e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


之后我们来测试一下把


![image.png](http://upload-images.jianshu.io/upload_images/2194379-87455475c9c79e21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

出现如下页面证明你的eclipse已经与tomcat关联成功了

![大功告成](http://upload-images.jianshu.io/upload_images/2194379-fa15a919f80cdfc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#    finally
#    enjoy jsp
#   write by objcat 2018.1.9






























