---
title: 玩转编程语言:基于Golang开发Web应用
date: 2018-04-27 17:39:48
tags:
categories: 工程技术
---
## 摘要

<!--more-->

## beego

#### Beego ABC

- lib
```go
go get github.com/astaxie/beego
go get github.com/beego/admin/src/lib
```

- tools
bee 可执行文件默认路径 $GOPATH/bin
```go
go get github.com/beego/bee  //tools
```

```bash
The commands are:
    new         create an application base on beego framework
    run         run the app which can hot compile
    pack        compress an beego project
    api         create an api application base on beego framework
    bale        packs non-Go files to Go source files
    version     show the bee & beego version
    generate    source code generator
    migrate     run database migrations

```

```bash
bash-3.2$ bee new hello
______
| ___ \
| |_/ /  ___   ___
| ___ \ / _ \ / _ \
| |_/ /|  __/|  __/
\____/  \___| \___| v1.9.1
```

#### Beego Resources
- [The architecture of Beego](https://beego.me/docs/intro/)
- [beego入门教程](https://github.com/beego/tutorial/blob/master/README_zh.md)
- [基于beego的后台管理系统](https://github.com/beego/admin)

## 参考文献
- [Top 6 web frameworks for Go as of 2017](https://blog.usejournal.com/top-6-web-frameworks-for-go-as-of-2017-23270e059c4b).doc   
