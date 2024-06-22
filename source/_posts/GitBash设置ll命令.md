---
title: GitBash设置ll命令
date: 2024-06-22 09:08:12
tags: 
    - "Git"
categories:
    - "Program"
cover: "/images/git.jpeg"
---
# GIT 配置 ll命令

## Windows 下

### 1 用户目录下

```shell
vi .bashrc
```

```shell
alias ll='ls -l'
```

```shell
source ~/.bashrc
```

## Linux 下

### 1 编辑 ~/.source

```shell
vi ~/.source
alias ll='ls -l'
```

### 2 生效

```shell
source ~/.bashrc
```