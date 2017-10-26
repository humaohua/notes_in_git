### docker容器
**docker容器基本操作**
```
启动容器
docker run IMAGE [COMMAND] [ARG...]
$ docker run ubuntu echo 'hello world'
```
```
启动交互式容器
$ docker run -i -t IMAGE /bin/bash
exit 退出交互式容器
```
```
查看容器
$ docker ps [-a][-l]
-a list all
-l list latest
```
```
查看容器配置信息, 可以是非运行容器
docker inspect 容器ID/名字
```
```
自定义容器名字
$ docker run --name=xxx -i -t ubuntu /bin/bash
```
```
重新启动已经停止的容器
$ docker start [-i] 容器名
```
```
删除停止的容器
$ docker rm 容器名
```
**守护式容器**
```
什么是守护式容器
* 能够长期运行
* 没有交互式会话
* 适合运行应用程序和服务
```
```
以守护形式运行容器
$ docker run -i -t IMAGE /bin/bash
Ctrl + P Ctrl + Q
$ docker attach id/name
```
```
启动守护式容器
$ docker run -d 镜像名 [COMMAND][ARG...]
$ docker run -d ubuntu /bin/sh -c "while true; do each hello world; sleep 1; done"
NOTE: -d 是以守护式运行容器, 但命令结束后, 依旧会停止, 使用循环保持容器运行
```
```
可以通过查看容器日志, 查看容器内部运行情况
$ docker logs [-f][-t][--tail]容器名
-f --follows=true|false default
是否跟随显示
-t --timestamps=true|false default
加上时间
-tail="all"
默认显示所有日志
```
```
运行中容器的进程
$  docker top name/id
```
```
在运行的容器中启动新进程
$ docker exec [-d][-i] 容器名 [COMMAND][ARG...]
```
```
停止守护式容器
$ docker stop 容器名(会等待)
$ docker kill 容器名
```
```
查看docker帮助文件
man docker -run
man docker -logs
man docker -top
man docker -exec
```










