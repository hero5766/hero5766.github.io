title: goproxy
author: hero576
tags:
  - golang
categories:
  - programme
date: 2020-07-21 16:23:00
---
> goproxy配置
<!--more-->

# 简介
- 随着Go 1.13发布，GOPROXY默认值proxy.golang.org在中国大陆不能被访问。
- go module公共代理仓库，代理并缓存go模块。你可以利用该代理来避免DNS污染导致的模块拉取缓慢或失败的问题，加速你的构建。
- 七牛云顺势推出`goproxy.cn`，以利于中国开发者更好使用Go Modules，它是非盈利性的项目

## 地址汇总
- https://mirrors.aliyun.com/goproxy/
- https://goproxy.cn

# 配置步骤
## 开启mod
```bash
go env -w GO111MODULE=on
```

## 配置环境变量
```bash
export GOPROXY=https://mirrors.aliyun.com/goproxy/
```

- 或者
```bash
go env -w GOPROXY=https://goproxy.cn,direct
```

