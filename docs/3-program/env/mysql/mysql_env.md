# 🍎 官方网站

https://www.mysql.com

# 🍎 Docker安装(推荐)

因为官网下载我目前比较喜欢实用的是docker安装, 分为挂载和非挂载

## 🌲 非挂载安装(学习使用)

非挂载就比较简单了, 直接一行命令, 最适合初学者使用

```shell
docker run -idt --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```

安装后查看一下

```
docker ps

CONTAINER ID   IMAGE       COMMAND                  CREATED      STATUS       PORTS                               NAMES
6eaf2027bee3   mysql:5.7   "docker-entrypoint.s…"   1 days ago   Up 2 hours   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql
```

## 🌲 挂载安装

所谓挂载安装就是把容器中的目录关联本机, 这样做的好处是当容器坏了的时候, 数据不会丢失, 因为是保存在本机的, 我们接下来就看一看吧, 首先是`docker-compose`的文件

```yml
version: '3'
services:
  mysql:
    image: mysql:5.7
    container_name: mysql
    command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=123456
      - TZ=Asia/Shanghai
    ports:
      - 3306:3306
    volumes:
      - ./mysql/data:/var/lib/mysql
      - ./mysql/log:/var/log/mysql
      - ./mysql/conf:/etc/mysql
```

然后是安装脚本

```shell
if [ -d "mysql" ];then
    echo "mysql文件夹已存在, 本脚本不支持覆盖安装!"
    exit
fi 
echo "step1: 拉取 mysql 5.7"
docker pull mysql:5.7
echo "step2: 启动预备容器 mysql 5.7"
docker run -idt --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
echo "step3: 容器启动成功, 开始拷贝conf和log"
mkdir mysql
docker cp mysql:/etc/mysql mysql/conf
docker cp mysql:/var/log/mysql mysql/log
echo "step3: 停止容器"
docker stop mysql
echo "step3: 移除容器"
docker rm mysql
echo "step4: 启动全新数据库"
docker-compose up -d
echo "step5: 启动成功"
```

使用方法是这样

![](images/Pasted%20image%2020230908110716.png)

在这个目录下运行`run.sh`即可启动数据库

# 🍎 传统安装

自己去官网下
