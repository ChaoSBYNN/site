---
title: Could not retrieve mirrorlist http://mirrorlist.centos.org
date: 2024-07-02 11:02:16
tags: 
    - "Linux"
categories:
    - "Program"
    - "Linux"
    - "CentOS"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/linux.png"
excerpt: "yum 使用时 http://mirrorlist.centos.org 无法请求"
---

> Could not retrieve mirrorlist http://mirrorlist.centos.org

## 替换 yum 软件源为阿里云镜像源

### 备份当前的 yum 软件源文件

在进行修改之前，首先备份原始的 yum 软件源配置文件。打开终端，输入以下命令备份：

```bash
sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```

### 编辑 yum 软件源配置文件

使用文本编辑器(如 `vim` 或 `nano`)打开 `/etc/yum.repos.d/CentOS-Base.repo` 文件：

```bash
sudo vim /etc/yum.repos.d/CentOS-Base.repo
```

### 注释掉原有的镜像源配置

在打开的文件中，找到 `[base]`、`[updates]`、`[extras]` 和 `[centosplus]` 等节，将它们的 `baseurl` 行注释掉(在行首添加 `#`符号)，同时保留 `mirrorlist` 行(如果有)。

例如:

```ini
[base]
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os

[updates]
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates

[extras]
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras

[centosplus]
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus

```

### 添加阿里云镜像源配置

```ini
[base]
name=CentOS-$releasever - Base - mirrors.aliyun.com
baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS-$releasever - Updates - mirrors.aliyun.com
baseurl=http://mirrors.aliyun.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

[extras]
name=CentOS-$releasever - Extras - mirrors.aliyun.com
baseurl=http://mirrors.aliyun.com/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

[centosplus]
name=CentOS-$releasever - Plus - mirrors.aliyun.com
baseurl=http://mirrors.aliyun.com/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7

```

* 将 `$releasever` 替换为您当前系统的 CentOS 版本号,如 `7` 或 `8`。
* 请确保 `gpgkey` 链接正确，以便验证下载的软件包。

### 保存并退出编辑器

在编辑完成后，保存文件并退出编辑器。

### 清理 yum 缓存

为了确保使用新的镜像源配置，清理当前的 yum 缓存

```bash
sudo yum update
```

### 恢复原始配置(可选)

如果需要恢复到原始的 CentOS 官方源，可以将之前备份的 `/etc/yum.repos.d/CentOS-Base.repo.bak` 文件复制回去，并清理 yum 缓存再次测试。
