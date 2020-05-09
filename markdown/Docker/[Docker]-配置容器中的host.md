# 前言
我们在配置服务器的时候经常会修改域名映射, 也就是`/etc/hosts`文件, 但是即便如此简单点的东西在网上写也是乱七八糟, 所以这里记录一个真正能用的方法.

# 正文

文章主要使用两种配置方法(命令配置和docker-compose配置):

##### 1.命令行配置 
`--add-host` 使用该参数可以配置多个host.

```
➜  Desktop docker run -it --add-host host1:192.168.1.1 --add-host host2:192.168.1.2 alpine
/ # cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
192.168.1.1	host1
192.168.1.2	host2
172.17.0.2	c91f741f3370
```

##### 2.docker-compose配置
 `extra_hosts:` 使用该参数可以配置多个host.
```
version: '2' 
services:
  service-nginx:
    image: nginx
    extra_hosts:
      - host1:192.168.1.1
      - host2:192.168.1.2
```

```
➜  Desktop docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
7004d8e742e9        nginx               "nginx -g 'daemon of…"   25 seconds ago      Up 3 seconds        80/tcp              desktop_service-alpine_1
➜  Desktop docker exec -it 7004d8e742e9 bash
root@7004d8e742e9:/# cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
192.168.1.1	host1
192.168.1.2	host2
172.20.0.2	7004d8e742e9
```
