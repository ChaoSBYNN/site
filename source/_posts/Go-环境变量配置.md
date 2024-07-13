---
title: Go 环境变量配置
date: 2024-07-01 08:55:17
tags: Go
categories:
    - "Program"
    - "Go"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/go/go.webp"
excerpt: "Go 环境变量配置"
---

## 获取安装包

`https://golang.org/dl/` 官网获取最新安装包

## Step 1 下载

```sh
wget https://go.dev/dl/go1.22.4.linux-amd64.tar.gz
```

## Step 2 解压

```sh
tar -C /usr/local -zxvf go1.22.4.linux-amd64.tar.gz
```

## Step 3 环境变量配置

```sh
vi /etc/profile
```

```sh
# 在/etc/profile最后一行添加
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
```

```sh
# 保存退出后source一下（vi 的使用方法可以自己搜索一下）
source /etc/profile
```

## Step 4 验证

```sh
go version
```