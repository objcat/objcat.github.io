MySQL在Mac电脑下使用是个坑, 好了下面跟着我们的镜头一起来看吧.

#一.安装MySQL
下载地址
https://www.mysql.com/

![](./images/2115ae402fd046213f3c29916e880a74/1.png)

![](./images/2115ae402fd046213f3c29916e880a74/2.png)

下载后安装 安装过程略.

#二.连接数据库
安装完成后我们会发现偏好设置里多了个MySQL
![](./images/2115ae402fd046213f3c29916e880a74/3.png)

点开后是这样的
![](./images/2115ae402fd046213f3c29916e880a74/4.png)

之后我们启动MySQL
![](./images/2115ae402fd046213f3c29916e880a74/5.png)


之后我们用Navicat连接一下MySQL

![](./images/2115ae402fd046213f3c29916e880a74/6.png)


然后我们会发现无法连接数据库
![](./images/2115ae402fd046213f3c29916e880a74/7.png)

###上面的提示说密码不对 因此我们需要重新设置数据库密码

#三.修改数据库密码

###1.首先关闭数据库
![](./images/2115ae402fd046213f3c29916e880a74/8.png)

###2.使用安全模式启动数据库
```
sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables
```
安全模式启动成功如下图
![](./images/2115ae402fd046213f3c29916e880a74/9.png)


###3.打开一个新窗口登陆MySQL
```
/usr/local/mysql/bin/mysql -u root
```
登陆成功如下图
![](./images/2115ae402fd046213f3c29916e880a74/10.png)


###4.修改数据库密码 密码为`234` 可以自行修改
```
UPDATE mysql.user SET authentication_string=PASSWORD('234') WHERE User='root';
```

修改成功如下图
![](./images/2115ae402fd046213f3c29916e880a74/11.png)
然后执行

```
FLUSH PRIVILEGES;
```

然后执行 \q 退出登录
```
\q
```

![](./images/2115ae402fd046213f3c29916e880a74/12.png)

![](./images/2115ae402fd046213f3c29916e880a74/13.png)

接下来我们停止MySQL服务
```
sudo /usr/local/mysql/support-files/mysql.server stop
```
![](./images/2115ae402fd046213f3c29916e880a74/14.png)


然后在偏好设置中手动启动MySQL
![](./images/2115ae402fd046213f3c29916e880a74/15.png)

启动完成后 我们在用Navicat连接一下

![](./images/2115ae402fd046213f3c29916e880a74/16.png)


![](./images/2115ae402fd046213f3c29916e880a74/17.png)


好我们发现成功了

![](./images/2115ae402fd046213f3c29916e880a74/18.png)


#finally enjoy it
#by objcat 2018.02.05













