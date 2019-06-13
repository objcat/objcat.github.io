MySQL在Mac电脑下使用是个坑, 好了下面跟着我们的镜头一起来看吧.

#一.安装MySQL
下载地址
https://www.mysql.com/

![image.png](http://upload-images.jianshu.io/upload_images/2194379-c77136dbe7a0a8d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/2194379-c24f743590872edc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下载后安装 安装过程略.

#二.连接数据库
安装完成后我们会发现偏好设置里多了个MySQL
![image.png](http://upload-images.jianshu.io/upload_images/2194379-dc584894d9afb737.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点开后是这样的
![image.png](http://upload-images.jianshu.io/upload_images/2194379-bd3ecdac3f036a82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后我们启动MySQL
![image.png](http://upload-images.jianshu.io/upload_images/2194379-6c1ddce91c340903.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


之后我们用Navicat连接一下MySQL

![image.png](http://upload-images.jianshu.io/upload_images/2194379-a77323d89f2bd2ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后我们会发现无法连接数据库
![image.png](http://upload-images.jianshu.io/upload_images/2194379-ef540cbb2de9f506.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###上面的提示说密码不对 因此我们需要重新设置数据库密码

#三.修改数据库密码

###1.首先关闭数据库
![image.png](http://upload-images.jianshu.io/upload_images/2194379-eaf5ef2a6fb9b7a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###2.使用安全模式启动数据库
```
sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables
```
安全模式启动成功如下图
![image.png](http://upload-images.jianshu.io/upload_images/2194379-9ea85adfd115c236.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###3.打开一个新窗口登陆MySQL
```
/usr/local/mysql/bin/mysql -u root
```
登陆成功如下图
![image.png](http://upload-images.jianshu.io/upload_images/2194379-4b34ff05713793fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###4.修改数据库密码 密码为`234` 可以自行修改
```
UPDATE mysql.user SET authentication_string=PASSWORD('234') WHERE User='root';
```

修改成功如下图
![image.png](http://upload-images.jianshu.io/upload_images/2194379-cbd6220bed30f1d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后执行

```
FLUSH PRIVILEGES;
```

然后执行 \q 退出登录
```
\q
```

![image.png](http://upload-images.jianshu.io/upload_images/2194379-07dc44a56b2c66e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/2194379-99615f9dbabef0ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来我们停止MySQL服务
```
sudo /usr/local/mysql/support-files/mysql.server stop
```
![image.png](http://upload-images.jianshu.io/upload_images/2194379-f95619a3350e901b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后在偏好设置中手动启动MySQL
![image.png](http://upload-images.jianshu.io/upload_images/2194379-44b5554d3b4e94fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

启动完成后 我们在用Navicat连接一下

![image.png](http://upload-images.jianshu.io/upload_images/2194379-287e5e73d5e30fc8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/2194379-739ec2223df4f2e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


好我们发现成功了

![image.png](http://upload-images.jianshu.io/upload_images/2194379-7b92333ce317ed36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#finally enjoy it
#by objcat 2018.02.05













