---
title: Linux 工具 htop 源码安装
date: 2024-07-26 12:32:49
tags: 
    - "Linux"
    - "Tools"
categories:
    - "Program"
    - "Linux"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/linux.png"
excerpt: "Linux 通过源码安装 htop"
---

![btop](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/linux/htop-2.0.png)

### 1. 准备环境

首先确保系统中已经安装了编译 `htop` 所需的工具和库：

```bash
sudo yum update
sudo yum install gcc-c++ ncurses-devel
```

### 2. 下载 `htop` 源码

从 `htop` 的 GitHub 仓库下载最新的源码包：

```bash
wget https://github.com/htop-dev/htop/archive/refs/tags/3.3.0.tar.gz -O htop-3.1.2.tar.gz
```

解压下载的源码包：

```bash
tar -xzvf htop-3.3.0.tar.gz
cd htop-3.3.0
```

### 3. 编译和安装

在源码目录中，执行以下命令编译和安装 `htop`：

```bash
./autogen.sh
./configure
make
sudo make install
```

### 4. 运行 `htop`

安装完成后，你可以通过以下命令启动 `htop`：

```bash
htop
```

### 注意事项

- 在编译之前，确保系统中已经安装了 `gcc`、`g++` 和 `ncurses` 库。
- 如果出现依赖项缺失的情况，根据错误信息安装相应的依赖项。
- 编译安装的 `htop` 可能需要在 `sudo` 下进行，以便将二进制文件复制到系统路径中。

通过这些步骤，你应该可以成功地从源码编译和安装 `htop`，并在 CentOS 系统上使用它来监控和管理系统进程。
