---
title: docker
tags:
  - skills
categories:
  - common
date: 2020-10-19 15:56:31
---
> 
<!-- more -->

# 简介
## 介绍[^1]
[^1]:[链接](https://www.runoob.com/docker/docker-architecture.html)
- Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。
- Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
- 容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

## 架构
- Docker 包括三个基本概念:

名称|说明
-|-
镜像（Image）|Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
容器（Container）|镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
仓库（Repository）|仓库可看成一个代码控制中心，用来保存镜像。

- Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。
- Docker 容器通过 Docker 镜像来创建。

![docker架构](/images/pasted-303.png)


概念|	说明
-|-
Docker 镜像(Images)|Docker 镜像是用于创建 Docker 容器的模板，比如 Ubuntu 系统。
Docker 容器(Container)|容器是独立运行的一个或一组应用，是镜像运行时的实体。
Docker 客户端(Client)|Docker 客户端通过命令行或者其他工具使用 Docker SDK (https://docs.docker.com/develop/sdk/) 与 Docker 的守护进程通信。
Docker 主机(Host)|一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。
Docker Registry|Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。<br>Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。<br>一个 Docker Registry 中可以包含多个仓库（Repository）；每个仓库可以包含多个标签（Tag）；每个标签对应一个镜像。<br>通常，一个仓库会包含同一个软件不同版本的镜像，而标签就常用于对应该软件的各个版本。我们可以通过 <仓库名>:<标签> 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签。
Docker Machine|Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。

## 安装
- [安装教程](https://www.runoob.com/docker/ubuntu-docker-install.html)，选择合适的操作系统

- 验证安装：`docker -v`
- 配置免sudo运行docker：
```bash
sudo groupadd docker # add dockergroup
sudo gpasswd -a ${USER} docker # add user to docker group
sudo service docker restart # reboot docker service
newgrp docker # change current user to new group
docker ps # ensure docker can run normally without sudo
```

## 更新镜像源
- docker官方源里的 Docker 版本比较旧，我们需要配置一个[国内源](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)，阿里的或者中科大的都可以。

- 修改配置文件：`$ sudo vim /etc/docker/daemon.json`
- 内容：
```json
{
	"registry-mirrors":["https://6kx4zyno.mirror.aliyuncs.com"]
}
```


# 容器
功能|命令|参数
-|-|-
启动容器|`docker run -it ubuntu /bin/bash`| -t: 在新容器内指定一个伪终端或终端。<br>-i: 允许你对容器内的标准输入 (STDIN) 进行交互。<br>加了 -d 参数容器会进入后台运行，默认不会进入容器，想要进入容器需要使用指令`docker exec`<br>-P:将容器内部使用的网络端口随机映射到我们使用的主机上。<br> -p:设置指定的映射端口：`-p 5000:5000`<br>--name: 标识来命名容器，没有指定docker会默认命名<br>-v：虚拟机和宿主机文件关联起来：`-v$(pwd):/data`
查看容器状态|`docker ps -a`| -a：查询所用容器，包括已停止的<br>-l:查询最后一次创建的容器
停止容器|`docker stop CONTAINERID/NAME`|
查看容器内的标准输出|`docker logs CONTAINERID/NAME`|-f: 让 docker logs 像使用 tail -f 一样来输出容器内部的标准输出。
指令查询帮助|`docker COMMAND --help`|`docker stats --help`
获取镜像|`docker pull ubuntu`|
启动已停止运行的容器|`docker start CONTAINERID/NAME`|
重启容器|`docker restart CONTAINERID/NAME`|
进入到正在运行的容器|`docker attach CONTAINERID/NAME`|使用`docker attach`如果从这个容器退出，会导致容器的停止。
在运行容器中运行命令|`docker exec CONTAINERID/NAME  /bin/bash`|推荐大家使用`docker exec`命令而不是attach，因为此退出容器终端，不会导致容器的停止。
将容器的文件系统导出为tar存档|`docker export CONTAINERID/NAME > ubuntu.tar`|导出容器快照到本地文件 ubuntu.tar。
导入容器|`cat ubuntu.tar|docker import - test/ubuntu:v1`<br>`docker import http://example.com/exampleimage.tgz example/imagerepo`|可以使用 docker import 从容器快照文件中再导入为镜像<br>也可以通过指定 URL 或者某个目录来导入
删除容器|`docker rm -f CONTAINERID/NAME`|
清理掉所有处于终止状态的容器|`docker container prune`|
查看网络端口与主机映射|`docker port CONTAINERID/NAME`|
容器容器运行的进程|`docker top CONTAINERID/NAME`|
显示一个或多个容器的详细信息|`docker inspect CONTAINERID/NAME`|返回一个 JSON 文件记录着 Docker 容器的配置和状态信息
从容器的更改创建一个新的映像|`docker commit`|
在容器和本地文件系统之间复制文件/文件夹|容器->本机`docker cp container_id:docker容器内的文件全路径 本机保存文件的全路径`<br>本机->容器：`docker cp E:\PHP\configure.txt 4a2f08d2c1f8:/data1/configure.txt`|
创建一个新的容器|`docker create`|
检查容器文件系统上文件或目录的更改|`docker diff`|
杀死一个或多个运行容器|`docker kill`|
列出容器|`docker ls`|-a：显示所有容器(默认只显示运行的)<br>-l：显示最新创建的容器(包括所有状态)<br>-q：只显示数字ID<br>-s：显示文件大小<br>--format：使用Go模板打印容器<br>--no-trunc：不要截断输出<br>-f：根据提供的条件过滤输出<br>-n	-1：显示最后创建的容器(包括所有状态)
暂停一个或多个容器内的所有进程|`docker pause`|
重命名容器|`docker rename`|
显示容器的实时流资源使用统计信息|`docker stats`|
取消暂停一个或多个容器内的所有流程|`docker unpause`|
更新一个或多个容器的配置|`docker update`|
阻止一个或多个容器停止，然后打印退出代码|`docker wait`|

## 运行容器方式
- 直接运行：`docker run ubuntu /bin/bash echo "hello"`，docker启动linux容器，并在容器中执行语句并输出到终端
- 交互式容器：`docker run -it ubuntu /bin/bash`
- 启动后台模式：`docker run -d ubuntu /bin/sh -c`，输出容器ID

## 容器状态

状态|说明
-|-
created|已创建
restarting|重启中
running|运行中
removing|迁移中
paused|暂停
exited|停止
dead|死亡


# 镜像
-当运行容器时，使用的镜像如果在本地中不存在，docker 就会自动从 docker 镜像仓库中下载，默认是从 Docker Hub 公共镜像源下载。 
- 同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本。如 ubuntu 仓库源里，有 15.10、14.04 等多个不同的版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。
- 如果你不指定一个镜像的版本标签，例如你只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像。

功能|命令|参数
-|-|-
镜像列表|`docker images`|
获取镜像|`docker pull ubuntu:13.10`|
查找镜像|`docker search httpd`|Docker Hub 网址为： https://hub.docker.com/
拖取镜像|`docker pull httpd`|
删除镜像|`docker rmi httpd`|

## 创建镜像
- 当我们从 docker 镜像仓库中下载的镜像不能满足我们的需求时，我们可以通过以下两种方式对镜像进行更改。
  - 1. 从已经创建的容器中更新镜像，并且提交这个镜像
  - 2. 使用 Dockerfile 指令来创建一个新的镜像

### 从已经创建的容器中更新镜像
功能|命令|参数
-|-|-
提交容器|`docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2`|返回sha256，-m: 提交的描述信息<br>-a: 指定镜像作者<br>e218edb10161：容器 ID<br>runoob/ubuntu:v2: 指定要创建的目标镜像名
查看新镜像|`docker images`|
使用新镜像启动容器|`docker run -t -i runoob/ubuntu:v2 /bin/bash`|

### 使用 Dockerfile 指令来创建
- 首先要创建`Dockerfile`文件：`vi Dockerfile`
```dockerfile
FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"
RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D
```

- 每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。
- 第一条FROM，指定使用哪个镜像源
- RUN 指令告诉docker 在镜像内执行命令，安装了什么。


功能|命令|参数
-|-|-
构建镜像|`docker build -t runoob/centos:6.7 .`|-t ：指定要创建的目标镜像名<br>. ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径
查看创建的镜像|`docker images`|
使用新镜像创建容器|`docker run -t -i runoob/centos:6.7  /bin/bash`|
设置镜像标签|`docker tag 860c279d2fec runoob/centos:dev`|

## 上传镜像
```bash
docker commit 8f # 提交
docker login #
docker tag aee hero576/mympc:v4  # 修改image的tag名称
docker push hero576/mympc:v4
```

## 连接
- Docker 容器互联：端口映射并不是唯一把 docker 连接到另一个容器的方法。docker 有一个连接系统允许将多个容器连接在一起，共享连接信息。docker 连接会创建一个父子关系，其中父容器可以看到子容器的信息。


功能|命令|参数
-|-|-
新建网络|`docker network create -d bridge test-net`| -d：参数指定 Docker 网络类型，有 bridge、overlay。其中 overlay 网络类型用于 Swarm mode，在本小节中你可以忽略它。
查询网络|`docker network ls`|
连接容器|运行一个容器并连接到新建的网络`docker run -itd --name test1 --network test-net ubuntu /bin/bash`<br>打开新的终端，再运行一个容器并加入到 test-net 网络:`docker run -itd --name test2 --network test-net ubuntu /bin/bash`<br>通过 ping 来证明 test1 容器和 test2 容器建立了互联关系:`ping test2`|

## DNS配置
**全局设置**
- 我们可以在宿主机的 /etc/docker/daemon.json 文件中增加以下内容来设置全部容器的 DNS
```json
{
  "dns" : [
    "114.114.114.114",
    "8.8.8.8"
  ]
}
```
- 设置后，启动容器的 DNS 会自动配置为 114.114.114.114 和 8.8.8.8。配置完，需要重启 docker 才能生效。
- 查看容器的 DNS 是否生效可以使用以下命令，它会输出容器的 DNS 信息：`docker run -it --rm  ubuntu  cat etc/resolv.conf`

**手动设置**
- 如果只想在指定的容器设置 DNS，则可以使用以下命令：
- `docker run -it --rm -h host_ubuntu  --dns=114.114.114.114 --dns-search=test.com ubuntu`
- 参数说明：
  - --rm：容器退出时自动清理容器内部的文件系统。
  - -h HOSTNAME 或者 --hostname=HOSTNAME： 设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts。
  - --dns=IP_ADDRESS： 添加 DNS 服务器到容器的 /etc/resolv.conf 中，让容器用这个服务器来解析所有不在 /etc/hosts 中的主机名。
  - --dns-search=DOMAIN： 设定容器的搜索域，当设定搜索域为 .example.com 时，在搜索一个名为 host 的主机时，DNS 不仅搜索 host，还会搜索 host.example.com。

# 仓库管理
- 仓库（Repository）是集中存放镜像的地方。
  - docker hub
  - [aliyun](cr.console.aliyun.com)

操作|说明
-|-
注册|在 https://hub.docker.com 免费注册一个 Docker 账号。
登录|`docker login`，登录需要输入用户名和密码，登录成功后，我们就可以从 docker hub 上拉取自己账号下的全部镜像。
退出 |`docker logout`
推送镜像|`docker tag ubuntu:18.04 username/ubuntu:18.04`<br>`docker push username/ubuntu:18.04`<br>`docker search username/ubuntu`

# Dockerfile
- Dockerfile 是一个用来构建镜像的文本文件，文本内容包含了一条条构建镜像所需的指令和说明。

## 使用 Dockerfile 定制镜像

- 新建Dockerfile文件
  - 在一个空目录下，新建一个名为 Dockerfile 文件，并在文件内添加以下内容：
  ```dockerfile
  FROM nginx 
  RUN echo '这是一个本地构建的nginx镜像' > /usr/share/nginx/html/index.html
  ```
- FROM指令
  - 定制的镜像都是基于 FROM 的镜像，这里的 nginx 就是定制需要的基础镜像。后续的操作都是基于 nginx。
- RUN指令
  - 用于执行后面跟着的命令行命令。有以下两种格式：
    **shell 格式：**
    ```text
    RUN <命令行命令>
    # <命令行命令> 等同于，在终端操作的 shell 命令。
    ```
    **exec 格式：**
    ```text
    RUN ["可执行文件", "参数1", "参数2"]
    # 例如：
    # RUN ["./test.php", "dev", "offline"] 等价于 RUN ./test.php dev offline
    ```

  - 注意：Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大。以 && 符号连接命令，这样执行后，只会创建 1 层镜像。

- 开始构建镜像：`docker build -t nginx:v3 .`

- 查看是否构建成功：`docker images`

- 上下文路径
  上下文路径，是指 docker 在构建镜像，有时候想要使用到本机的文件（比如复制），docker build 命令得知这个路径后，会将路径下的所有内容打包。 
  > 解析：由于 docker 的运行模式是 C/S。我们本机是 C，docker 引擎是 S。实际的构建过程是在 docker 引擎下完成的，所以这个时候无法用到我们本机的文件。这就需要把我们本机的指定目录下的文件一起打包提供给 docker 引擎使用。
  - 如果未说明最后一个参数，那么默认上下文路径就是 Dockerfile 所在的位置。
  > 注意：上下文路径下不要放无用的文件，因为会一起打包发送给 docker 引擎，如果文件过多会造成过程缓慢。

# 问题
## docker 权限问题 Got permission denied while trying to connect to the Docker daemon socket at 。。。
- 在用户权限下docker 命令需要 sudo 否则出现以下问题
通过将用户添加到docker用户组可以将sudo去掉，命令如下

```bash
sudo groupadd docker #添加docker用户组
sudo gpasswd -a $USER docker #将登陆用户加入到docker用户组中
newgrp docker #更新用户组
```

## docker部署nginx
- [链接](https://www.cnblogs.com/saneri/p/11799865.html)
```bash
# 查找 Docker Hub 上的 nginx 镜像
docker search nginx
# 拉取官方的Nginx镜像
docker pull nginx
# 在本地镜像列表里查到镜像
docker images nginx
```

- 使用 NGINX 默认的配置来启动一个 Nginx 容器实例：
```bash
docker run --rm --name nginx-test -p 8080:80 -d nginx
```

参数|说明
-|-
--rm|容器终止运行后，自动删除容器文件。
--name nginx-test|容器的名字叫做nginx-test,名字自己定义.
-p| 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口
-d|容器启动后，在后台运行

```bash
#查看启动的docker容器
docker container ps
```

- 在浏览器中打开http://172.17.0.1:8080就可以看到nginx了

**映射本地目录**

- 创建本地目录，用于存放Nginx的相关文件信息.
```bash
mkdir -p /home/nginx/www /home/nginx/logs /home/nginx/conf
#www: 目录将映射为 nginx 容器配置的虚拟目录。
#logs: 目录将映射为 nginx 容器的日志目录。
#conf: 目录里的配置文件将映射为 nginx 容器的配置文件。
```

- 拷贝容器内 Nginx 默认配置文件到本地当前目录下的 conf 目录,容器ID可以查看 docker ps 命令输入中的第一列：
```bash
docker ps
docker cp CONTAINER_ID_xx:/etc/nginx/nginx.conf /home/nginx/conf/
```

- 部署命令
```bash
docker run --rm -d -p 8081:80 --name nginx-test-web \
  -v /home/nginx/www:/usr/share/nginx/html \
  -v /home/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
  -v /home/nginx/logs:/var/log/nginx \
  nginx
```

参数|说明
-|-
--rm|容器终止运行后，自动删除容器文件。
-p 8081:80|将容器的 80 端口映射到主机的 8082 端口.
--name nginx-test-web|将容器命名为 nginx-test-web 
-v /home/nginx/www:/usr/share/nginx/html|将我们自己创建的 www 目录挂载到容器的 /usr/share/nginx/html。
-v /home/nginx/conf/nginx.conf:/etc/nginx/nginx.conf|将我们自己创建的 nginx.conf 挂载到容器的 /etc/nginx/nginx.conf。
-v /home/nginx/logs:/var/log/nginx|将我们自己创建的 logs 挂载到容器的 /var/log/nginx。


- 启动以上命令后进入 /home/nginx/www 目录：
```bash
cd /home/nginx/www/
vim index.html 
```

- 在浏览器里面输入http://172.17.0.1:8081/
- 如果在访问时出现403错误，应该是index.html文件权限不足，给成644就行.

## docker下使用opencv

```bash
docker pull ubuntu
# 开启容器
docker run -it --name opencv4_3d -v /mnt/d/code_basket/c/reconstruction:/home/workspace ubuntu
# 退出后重新进入容器
docker exec -it /bin/bash
```

## 提交镜像
- [链接](https://www.cnblogs.com/codehu/p/docker_pull_push.html)

```bash
docker commit -p -a "hero576" -m "for opencv4 reconstruction" opencv4_3d hero576/opencv4:v1
```

参数|说明
-|-
-a |提交的镜像作者；
-m |提交时的说明文字；
-p |在commit时，将容器暂停。

- 再次查看镜像，发现了本地新提交的codehi/nginx:v1的镜像
```bash
docker images
```

- 然后登陆到`docker hub`
```bash
docker login
```

- 将镜像推送到`docker hub`仓库中
```bash
docker push hero576/opencv4:v1
```

- 在另一台服务器使用pull方法下载这个镜像
```bash
docker pull hero576/opencv4:v1
```
**push 时报错 "denied: requested access to the resource is denied" 的解决方法**
- 报这个错说明tag需要改名字，由于之前commit的时候没有填tag，导致这块上传不上去，解决办法：
```bash
docker tag 1d8fca63675a hero576/opencv4:v1
```

## 解决docker容器内中文乱码问题
**临时解决**
locale -a查看容器所有语言环境

```bash
# 进入容器
docker exec -it 容器ID bash
[root@41056165bd6f /]#  locale -a
C
C.utf8
POSIX
```

- C.UTF-8可以支持中文，只需要把容器编码设置为C.UTF-8即可
```bash
[root@41056165bd6f /]# vim /etc/profile
# 添加 LANG=C.UTF-8
[root@41056165bd6f /]# source /etc/profile
```
- 不过这个方法，有个弊端，容器kill掉之后，重新启动容器，需要再次配置；


**永久解决**
- 修改Dockerfile
```bash
ENV LANG C.UTF-8
```

- 然后重新生成镜像，重新启动容器
- 这样生成的镜像，就已经解决了乱码问题

```bash
cat << EOF > /root/.vimrc
:set encoding=utf-8
:set fileencodings=ucs-bom,utf-8,cp936
:set fileencoding=gb2312
:set termencoding=utf-8
EOF
```







