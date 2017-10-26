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
* Docker端口映射

```
$ docker run [-P 随机映射所有端口][-p 仅映射指定端口]
* -p
1 containerPort
	docker run -p 80 -i -t ubuntu /bin/bash
2 hostPort:containerPort
	docker run -p 8080:80 -i -t ubuntu /bin/bash
3 ip:containerPort
	docker run -p 0.0.0.0:80 -i -t ubuntu /bin/bash
4 ip:hostPort:containerPort
	docker run -p 0.0.0.0:8080:80 -i -t ubuntu /bin/bash
```
* Nginx部署流程

```
* 创建映射80端口的交互式容器
	docker run -p 80 --name web -i -t ubuntu /bin/bash
* 安装Nginx
	apt-get install -y nginx
* 安装文本编辑器vim
	apt-get install -y vim
* 创建静态页面
	mkdir -p /var/www/html
	cd /var/www/html
	vim index.html
* 运行Nginx
	whereis nginx
	ls /etc/nginx
	vim /etc/nginx/sites-enabled/default
	修改 root 为 /var/www/html;
	cd /
	nginx
	ps -ef
	ctrl + p
	docker ps
	docker port web
	docker top web
* 验证网站访问
	curl http://127.0.0.1:4317
	docker inspect web
	curl http://x.x.x.z
	docker stop web
	docker start -i web
	ctrl + p
	docker exec web nginx
	停止一个容器并重新启动时,原来分配的IP及端口都会改变
```










