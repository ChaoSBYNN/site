---
title: Linux 防火墙端口开放
date: 2024-06-22 09:10:12
tags: 
    - "Linux"
categories:
    - "Program"
cover: "/images/linux.png"
---

## 端口开放

在Linux中，要开放一个端口，通常需要使用iptables或者firewalld（如果安装了firewalld的话）。

以下是两种情况的示例：

1. 使用iptables开放端口（例如开放TCP端口8080）：

```shell
sudo iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
```

2. 如果你的系统使用firewalld，可以使用以下命令：

```shell
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
```

请确保替换8080为你想要开放的实际端口号，并根据你的实际情况选择使用iptables还是firewalld。

注意：如果你的系统使用的是ufw，可以使用以下命令：

```shell
sudo ufw allow 8080/tcp
```

这些命令将允许进入指定端口的流量。如果你需要指定IP范围或者其他更复杂的规则，你可能需要编辑iptables的规则或者使用firewalld的更高级功能。