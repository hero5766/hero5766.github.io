---
title: supervisor
author: hero576
tags:
  - linux
categories:
  - programme
date: 2020-08-03 22:03:00
---

> supervisor简单使用

<!--more-->

# 简介
- Supervisor是一个用Python2写的进程管理工具，可以很方便的对进程进行启动、停止、重启等操作。

- [官方网址](http://supervisord.org/)

# 安装
- 安装成功后，supervisor就会默认启动，在/etc/supervisor目录下，生成supervisord.conf配置文件。
```bash
sudo apt-get install supervisor
```

- 需要注意的是，如果不是root账号，需要对这些目录进行权限设置，要不然会报一些错误（一定要在 root 账号下进行配置）。
```bash
sudo chmod 777 /var/run
sudo chmod 777 /etc/supervisor
```

- 接着就可以启动 Supervisord 了：
```bash
supervisord
```

# 配置
- 样例
```ini
[program:meta.txn.recover.on.error] ;配置监控程序名称
command=/cas/bin/meta.sh      ; 启动被监控的进程命令
numprocs=1                    ; 启动几个进程
directory=/cas/bin            ; 执行前要不要先cd到目录去，一般不用
autostart=true                ; 随着supervisord的启动而启动
autorestart=true              ; 自动重启。。当然要选上了
startretries=10               ; 启动失败时的最多重试次数
exitcodes=0                   ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
stopsignal=KILL               ; 用来杀死进程的信号
stopwaitsecs=10               ; 发送SIGKILL前的等待时间
redirect_stderr=true          ; 重定向stderr到stdout
stdout_logfile=logfile        ; 指定日志文件，会在执行目录创建日志文件
```

|配置|说明|
|-|-|



# 常用命令

|命令|说明|
|-|-|
|supervisorctl start programxxx|启动某个进程|
|supervisorctl restart programxxx|重启某个进程|
|supervisorctl stop groupworker: |重启所有属于名为groupworker这个分组的进程(start,restart同理)|
|supervisorctl stop all|停止全部进程，注：start、restart、stop都不会载入最新的配置文件。|
|supervisorctl reload|载入最新的配置文件，停止原有进程并按新的配置启动、管理所有进程。|
|supervisorctl update|根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启。|
|supervisorctl|查看正在守候的进程|

- 注意：显式用stop停止掉的进程，用reload或者update都不会自动重启
- supervisor启动和停止的日志文件存放在/var/log/supervisor/supervisord.log


# 问题汇总
## error:class 'socket.error' [Errno 2] No such file or directory: file: /usr/lib64/python2.7/socket

- 问题：supervisor 配置完毕，使用supervisorctl reload 和supervisorctl update 启动时候报错
- 解决方法：使用下面命令启动
```bash
/usr/bin/python2 /usr/bin/supervisord -c /etc/supervisor/supervisord.conf
```

