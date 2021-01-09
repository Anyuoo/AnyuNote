# docker

## 1.0docker常用命令

> docker ps 和 rm

```bash
#查看容器 -a 所有容器 -q 只显示容器编号 -n 最近创建的n个容器
docker ps 
#删除所有容器
docker rm $(docker ps -aq)
#删除所有镜像
docker rmi $(docker images -q)
docker rmi $(docker images -q) -f
```

> docker run 

```bash
#运行容器 -d 后台运行； -it 交互式运行，并打开新的模拟端口； --name 容器名称 -p 端口号
docker run [-d/-it] [--name example] [-p 80] image_name [ags]
```



## 2.0 docker构建镜像

1. `docker commit`  基于容器创建镜像

```bash
#-a 'author' -m 'commit message' 容器名 镜像名
docker commit -a 'anyu' -m 'ngnix' commit_test anyu/commit_test
#运行创建的容器，Nginx易前台的方式运行
docker run  -d --name nginx_web1 anyu/commit_test nginx -g "daemon off;"
#制定端口号
docker run -d --name nginx_web02 -p 80 anyu/commit_test nginx -g "daemon off;"
```

2. `dockerfile 否建镜像`

```bash
#
```

## 3.0 docker的c/s模式

<img src="C:\Users\Administrator\Desktop\note\docker\捕获.PNG" style="zoom:80%;" />

<img src="C:\Users\Administrator\Desktop\note\docker\2.PNG" alt="链接方式" style="zoom:100%;" />

> docker  socket 链接 ,remote request

```bash
#netcat  -U 
nc -U /var/run/docker.sock
GET /info HTTP/1.1
```

## 4.0 docker守护进程

> 查看守护进程

```bash
#查看守护进程
ps -ef | grep docker
sudo status docker
```

> 对守护进程操作

```ba
service docker stop
service docker start
service docker restart
```

![](C:\Users\Administrator\Desktop\note\docker\docker的启动选项.PNG)

## 5.0 docker远程访问

## 6.0 dockerfile

1. FROM 
2. MAINTAINER <name>
3. RUN 指定当前镜像中运行的指令 ，分层指令有先后
4. EXPOSE <port> 指定端口 
5. CMD 容器运行的指令（docker run 指令可以覆盖）
6. ENTYPOINT （docker run 指令不可以覆盖）
7. COPY \ADD
8. WORKDIR
9. USER daenmon
10. ONBUILD 镜像触发器

![](C:\Users\Administrator\Desktop\note\docker\run.PNG)

![](C:\Users\Administrator\Desktop\note\docker\run1.PNG)

![](C:\Users\Administrator\Desktop\note\docker\dockerfile构建过程.PNG)

## 7.0 docker 网络

> 网桥 数据链路层

```bash
brctl show #查看网桥
```

