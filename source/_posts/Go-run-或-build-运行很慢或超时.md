---
title: Go run 或 build 运行很慢或超时
date: 2024-07-01 09:04:30
tags: Go
categories:
    - "Program"
    - "Go"
cover: "/images/go/go.webp"
excerpt: "Go 默认镜像地址访问超时"
---

## Step 1 查看环境变量

```sh
go env
```

```sh
GOOS='linux'
GOPATH='/root/go'
GOPRIVATE=''
# 这里镜像问题
GOPROXY='https://proxy.golang.org,direct'
GOROOT='/usr/local/go'
```

## Step 2 修改镜像地址

```sh
go env -w GOPROXY=http://mirrors.aliyun.com/goproxy/
```

> `-w` 标记 要求一个或多个形式为 `NAME=VALUE` 的参数, 并且覆盖默认的设置