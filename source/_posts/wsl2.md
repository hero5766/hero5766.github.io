title: wsl2
author: hero576
date: 2020-07-07 21:46:52
tags:
---
> windows对linux的支持
<!--more-->

- [教程链接](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)

- 如果电脑上删除了windows应用商店，可以用如下方法重新安装：
  - Windows+x选A，以管理员身份运行WindowsPowerShell
  - 输入命令`Get-AppxPackage -allusers | Select Name, PackageFullName`，获取当前系统安装的所有应用。
  - 在列表中找到名称为“Microsoft.WindowsStore”(即应用商店)的应用，然后复制右侧对应的包名称。`Microsoft.WindowsStore_11801.1001.6.0_x64__8wekyb3d8bbwe`
  - 输入下列命令进行应用商店的重装（版本不同包名称也不同，下列命令看着替换一下自己的包名称就好了。）：`Add-appxpackage -register "C:\Program Files\WindowsApps\Microsoft.WindowsStore_11801.1001.6.0_x64__8wekyb3d8bbwe\appxmanifest.xml" -disabledevelopmentmode`
  - 打开开始菜单，应用商店已装回。


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