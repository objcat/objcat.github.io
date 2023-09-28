# 🍎 官方网站

https://www.mysql.com

# 🍎 苹果M1/ARM平台

因为很多docker镜像都不支持arm, 所以可能出现下面的提示

```
no matching manifest for linux/arm64/v8 in the manifest list entries
```

不要沮丧, 换个版本即可

比如我们的mysql5可能不支持`m1`, 我们就可以尝试安装`mysql8`

# 🍎 Docker安装MySQL(推荐)

因为官网下载我目前比较喜欢实用的是docker安装, 分为挂载和非挂载

## 🌲 5.7

### 🌸 快速安装(非挂载)

非挂载就比较简单了, 直接一行命令, 最适合初学者使用, 用户名`root`, 密码`123456`

```
docker pull mysql:5.7
```

```shell
docker run -idt --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```

安装后查看一下

```
docker ps

CONTAINER ID   IMAGE       COMMAND                  CREATED      STATUS       PORTS                               NAMES
6eaf2027bee3   mysql:5.7   "docker-entrypoint.s…"   1 days ago   Up 2 hours   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql
```

### 🌸 快速安装(挂载)

白猫个人镜像仓库, 拉取速度快

```shell
curl -sS https://gitee.com/jskk/test-docker-compose/raw/master/mysql/5_7/auto_baimao.sh | sh

curl -sS https://gitee.com/jskk/test-docker-compose/raw/master/mysql/5_7/auto.sh | sh
```

### 🌸 docker-compose

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

## 🌲 8.0

### 🌸 快速安装(非挂载)

非挂载就比较简单了, 直接一行命令, 最适合初学者使用, 用户名`root`, 密码`123456`

```shell
docker run -idt --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:8
```

### 🌸 快速安装(挂载)

白猫个人镜像仓库, 拉取速度快

```shell
curl -sS https://gitee.com/jskk/test-docker-compose/raw/master/mysql/8_0/auto_baimao.sh | sh

curl -sS https://gitee.com/jskk/test-docker-compose/raw/master/mysql/8_0/auto.sh | sh
```

### 🌸 docker-compose

我们知道了路径就可以写`compose`了

```yml
version: '3'
services:
  mysql:
    image: mysql:8.0.27
    container_name: mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=123456
      - TZ=Asia/Shanghai
    ports:
      - 3306:3306
    volumes:
      - ./mysql/data:/var/lib/mysql
      - ./mysql/conf:/etc/mysql
      # - ./mysql/log:/var/log/mysql
      # - ./mysql/mysql-files:/lib/mysql-files/
```

### 🌸 填坑

8.0和5.7有些不一样, 容器里默认配置文件的路径改了, log也不见了, 所以我们要先启动容器来看一看, 进入后我们查找配置文件的位置

```
docker run -idt --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:8
```

然后我们进入容器

```
docker exec -it mysql bash
```

进入后我们直接查看配置文件路径

```shell
mysql --help | grep my.cnf

order of preference, my.cnf, $MYSQL_TCP_PORT,

/etc/my.cnf /etc/mysql/my.cnf /usr/etc/my.cnf ~/.my.cnf
```

我们可以看到好几个路径, 到底哪个是真的呢, 我们先去最熟悉的`/etc/mysql/`

```
bash-4.4# cd /etc/mysql/       
bash-4.4# ls
conf.d
bash-4.4# cd conf.d/
bash-4.4# ls
```

我们发现这里面除了一个`conf.d`文件夹没有任何东西, 然后我试了后面那几个都不行, 只有最前面的可以

```
bash-4.4# cat /etc/my.cnf
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/8.1/en/server-configuration-defaults.html
[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
# Remove leading # to revert to previous value for default_authentication_plugin,
# this will increase compatibility with older clients. For background, see:
# https://dev.mysql.com/doc/refman/8.1/en/server-system-variables.html#sysvar_default_authentication_plugin
# default-authentication-plugin=mysql_native_password
skip-host-cache
skip-name-resolve
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock
secure-file-priv=/var/lib/mysql-files
user=mysql
pid-file=/var/run/mysqld/mysqld.pid
[client]
socket=/var/run/mysqld/mysqld.sock
!includedir /etc/mysql/conf.d/
```

除了配置文件路径我们还要查看log路径, 但是这个路径我一直没找到, 所以暂时放弃了

# 🍎 传统安装

自己去官网下
