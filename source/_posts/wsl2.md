title: wsl2
author: hero576
date: 2020-07-07 21:46:52
tags:
---
> windows对linux的支持
<!--more-->

- [教程链接](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)


## 安装docker
- [安装命令](http://mirror.azure.cn/help/docker-engine.html)
```
curl -skSL https://mirror.azure.cn/repo/install-docker-ce.sh | sh -s -- --mirror AzureChinaCloud
```

- 启动docker
```
sudo service docker start
```

- 拉镜像
```
sudo docker pull nginx
```

- 配置端口映射
```
sudo docker run --name nginx:latest -p 8080:80 -d nginx
```

- 在windows访问`http://localhost:8080`