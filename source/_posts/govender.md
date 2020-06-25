title: go使用vendor管理目录
author: hero576
tags:
  - golang
categories:
  - programme
date: 2020-06-06 12:48:00
---
> golang的依赖包默认是从`GOROOT`和`GOPATH`去找，vendor可以在项目众建立`vendor`目录，方便不同机器直接使用所依赖的包。
<!--more-->

# 介绍
- `vendor`目录允许不同的代码库拥有它自己的依赖包，并且不同于其他代码库的版本，这就很好的做到了工程的隔离。
- 随着Go 1.5 release版本的发布，vendor目录被添加到除了GOPATH和GOROOT之外的依赖目录查找的解决方案。,但需要手动设置环境变量 GO15VENDOREXPERIMENT=1
- 在发布 1.6 版本时，该环境变量的值已经默认设置为 1 了，该值可以使用 go env 命令查看。
- 在发布 1.7 版本时，已去掉该环境变量，默认开启 vendor 特性。
- **注意**：即使使用vendor，也必须在GOPATH中，在go的工具链中，你逃不掉GOPATH的
- 查找依赖包路径顺序
  - 当前包下的vendor目录。
  - 向上级目录查找，直到找到src下的vendor目录。
  - 在GOPATH下面查找依赖包。
  - 在GOROOT目录下查找
- 建议
  - 一个库工程（不包含main的package）不应该在自己的版本控制中存储外部的包在vendor\目录中，除非他们有特殊原因并且知道为什么要这么做。
  - 在一个应用中，（包含main的package），建议只有一个vendor目录在代码库一级目录。

# 使用
## 安装
- 在终端执行安装命令即可
```
go get -u github.com/kardianos/govendor
```

- 为了方便快捷使用 govendor，建议将 $GOPATH/bin 添加到 PATH 中。Linux/macOS 如下设置：
```
export PATH="$GOPATH/bin:$PATH"
```

## 项目使用
- 初始化：在项目根目录下执行以下命令进行 vendor 初始化：
```
govendor init
```
  - 项目根目录下即会自动生成 vendor 目录和 vendor.json 文件。此时 vendor.json 文件内容为：
```
{
	"comment": "",
	"ignore": "test",
	"package": [],
	"rootPath": "govendor-example"
}
```

- 将已被引用且在 $GOPATH 下的所有包复制到 vendor 目录
```
govendor add +external
govendor add +e //简写
```
- 仅从 $GOPATH 中复制指定包
```
govendor add gopkg.in/yaml.v2
```

- 列出代码中所有被引用到的包及其状态
```
govendor list
```

- 列出一个包被哪些包引用
```
govendor list -v fmt
```

- 从远程仓库添加或更新某个包(不会在 $GOPATH 也存一份)
```
govendor fetch golang.org/x/net/context
```

- 安装指定版本的包
```
govendor fetch golang.org/x/net/context@a4bbce9fcae005b22ae5443f6af064d80a6f5a55
govendor fetch golang.org/x/net/context@v1   # Get latest v1.*.* tag or branch.
govendor fetch golang.org/x/net/context@=v1  # Get the tag or branch named "v1".
```

- 只格式化项目自身代码(vendor 目录下的不变动)
```
govendor fmt +local
```

- 只构建编译项目内部的包
```
govendor install +local
```

- 只测试项目内部的测试案例
```
govendor test +local
```

- 构建所有 vendor 包
```
govendor install +vendor,^program
```

- 拉取所有依赖的包到 vendor 目录(包括 $GOPATH 存在或不存在的包)
```
govendor fetch +out
```

- 包已在 vendor 目录，但想从 $GOPATH 更新
```
govendor update +vendor
```

- 已修改了 $GOPATH 里的某个包，现在想将已修改且未提交的包更新到 vendor
```
govendor update -uncommitted <updated-package-import-path>
```

- Fork 了某个包，但尚未合并，该如何引用到最新的代码包
```
govendor fetch github.com/normal/pkg::github.com/myfork/pkg
```

  - 此时将从 myfork 拉取代码，而不是 normal。
- vendor.json 中记录了依赖包信息，该如何拉取更新
```
govendor sync
```

## govendor 子命令
- 各子命令详细用法可通过 govendor COMMAND -h 或阅读[官方链接](github.com/kardianos/govendor/context)查看源码包如何实现的。

|子命令|	功能|
|--|--|
|init|	创建 vendor 目录和 vendor.json 文件|
|list|	列出&过滤依赖包及其状态|
|add|	从 $GOPATH 复制包到项目 vendor 目录|
|update|	从 $GOPATH 更新依赖包到项目 vendor 目录|
|remove|	从 vendor 目录移除依赖的包|
|status|	列出所有缺失、过期和修改过的包|
|fetch|	从远程仓库添加或更新包到项目 vendor 目录(不会存储到 $GOPATH)|
|sync|	根据 vendor.json 拉取相匹配的包到 vendor 目录|
|migrate|	从其他基于 vendor 实现的包管理工具中一键迁移|
|get|	与 go get 类似，将包下载到 $GOPATH，再将依赖包复制到 vendor 目录|
|license|	列出所有依赖包的 LICENSE|
|shell|	可一次性运行多个 govendor 命令|

## govendor 状态参数
- 支持状态参数的子命令有：list、add、update、remove、fetch

|状态|	缩写|	含义|
|--|--|
|+local|	l|	本地包，即项目内部编写的包|
|+external|	e|	外部包，即在 GOPATH 中、却不在项目 vendor 目录|
|+vendor|	v|	已在 vendor 目录下的包|
|+std|	s|	标准库里的包|
|+excluded|	x|	明确被排除的外部包|
|+unused|	u|	未使用的包，即在 vendor 目录下，但项目中并未引用到|
|+missing|	m|	被引用了但却找不到的包|
|+program|	p|	主程序包，即可被编译为执行文件的包|
|+outside|	|	相当于状态为 +external +missing|
|+all|	|	所有包|

# Go modules
- 普大喜奔的是，从 Go 1.11 版本开始，官方已内置了更为强大的 Go modules 来一统多年来 Go 包依赖管理混乱的局面(Go 官方之前推出的 dep 工具也几乎胎死腹中)，并且将在 1.13 版本中正式默认开启。
- 目前已受到社区的看好和强烈推荐，建议新项目采用 Go modules。

[原文链接](https://shockerli.net/post/go-package-manage-tool-govendor/#go-modules)