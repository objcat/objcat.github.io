
#一.安装tomcat

- step 1 下载tomcat服务器
无脑点开
http://tomcat.apache.org
![](./images/ab085d17670112a21fa1c4762f8739c5/1.png)
![](./images/ab085d17670112a21fa1c4762f8739c5/2.png)

- step 2 安装
zip 解压出来是这么个东西
![](./images/ab085d17670112a21fa1c4762f8739c5/3.png)

进入bin目录可以看到两个文件
![](./images/ab085d17670112a21fa1c4762f8739c5/4.png)

这两个文件就是启动和关闭服务器用的

下面我们cd到bin目录下 来启动服务器
```
cd /Users/hfcb/Downloads/apache-tomcat-9.0.2/bin 
sudo sh startup.sh
```

会出现如下错误
![](./images/ab085d17670112a21fa1c4762f8739c5/5.png)


因此我们需要给文件权限
```
chmod 777 *.sh
```

这里再次运行 
```
sudo sh startup.sh
```
我们发现服务器可以正常启动了 出现如下命令行
![](./images/ab085d17670112a21fa1c4762f8739c5/6.png)


我们在web访问以下试试吧 出现如下页面即为成功启动服务器
http://localhost:8080
![](./images/ab085d17670112a21fa1c4762f8739c5/7.png)


接下来我们点击 Server Status
![](./images/ab085d17670112a21fa1c4762f8739c5/8.png)

会出现输入用户名和密码  这里输入 
```
用户名: tomcat  
密码:tomcat
```
![](./images/ab085d17670112a21fa1c4762f8739c5/9.png)

点击login

结果发现无法登陆  弹出一个空白的登陆窗口
![](./images/ab085d17670112a21fa1c4762f8739c5/10.png)


这时我们点击取消会出现401画面

![](./images/ab085d17670112a21fa1c4762f8739c5/11.png)

我们复制红框中的文字 打开如图所示目录下的文件
![](./images/ab085d17670112a21fa1c4762f8739c5/12.png)

![](./images/ab085d17670112a21fa1c4762f8739c5/13.png)

我们会看到如下页面 把文字复制到最下边 并修改用户名和密码都为 tomcat
![](./images/ab085d17670112a21fa1c4762f8739c5/14.png)


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
![](./images/ab085d17670112a21fa1c4762f8739c5/15.png)

出现如下页面即为成功
![](./images/ab085d17670112a21fa1c4762f8739c5/16.png)



#一.安装eclipse
无脑点开 下载
https://www.eclipse.org/downloads/
![](./images/ab085d17670112a21fa1c4762f8739c5/17.png)

下载之后安装

在此之前请确保你已经安装过java环境  如果没安装去java下载一个jdk
http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html

#安装过程略....


安装之后打开eclipse  cmd + , (cmd + 逗号) 打开设置页面 点击runtime Environments

![](./images/ab085d17670112a21fa1c4762f8739c5/18.png)

![](./images/ab085d17670112a21fa1c4762f8739c5/19.png)


![](./images/ab085d17670112a21fa1c4762f8739c5/20.png)

选择自己的服务器路径  finished
![](./images/ab085d17670112a21fa1c4762f8739c5/21.png)


![](./images/ab085d17670112a21fa1c4762f8739c5/22.png)


之后我们新建一个项目
![](./images/ab085d17670112a21fa1c4762f8739c5/23.png)


![](./images/ab085d17670112a21fa1c4762f8739c5/24.png)

点击next

![](./images/ab085d17670112a21fa1c4762f8739c5/25.png)


next

![](./images/ab085d17670112a21fa1c4762f8739c5/26.png)


next

钩一定要钩上

![](./images/ab085d17670112a21fa1c4762f8739c5/27.png)


之后我们新建一个jsp文件
![](./images/ab085d17670112a21fa1c4762f8739c5/28.png)

![](./images/ab085d17670112a21fa1c4762f8739c5/29.png)

之后我们会发现报一个错11

![](./images/ab085d17670112a21fa1c4762f8739c5/30.png)

我们需要去配置一下 Build Path

![](./images/ab085d17670112a21fa1c4762f8739c5/31.png)

![](./images/ab085d17670112a21fa1c4762f8739c5/32.png)

![](./images/ab085d17670112a21fa1c4762f8739c5/33.png)


之后我们来测试一下把


![](./images/ab085d17670112a21fa1c4762f8739c5/34.png)

出现如下页面证明你的eclipse已经与tomcat关联成功了

![](./images/ab085d17670112a21fa1c4762f8739c5/35.png)



#    finally
#    enjoy jsp
#   write by objcat 2018.1.9






























