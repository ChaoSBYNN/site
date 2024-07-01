---
title: Linux 将可执行文件设为全局可用
date: 2024-07-01 08:46:05
tags: Linux
categories:
    - "Program"
    - "Linux"
cover: "/images/linux.png"
excerpt: "Linux 源码安装的程序 全局执行"
---

## Step 1

```sh
sudo ln -s <binary-path>/<binary-name> /usr/bin/<binary-name>
```

或

```sh
sudo cp <binary-name> /usr/local/bin/
```

## Step 2

```sh
echo "export PATH=$PATH:/usr/local/<binary-name>" >> ~/.profile && source ~/.profile
```


## 补充

* `/usr/local/bin/` 用户安装的
* `/usr/bin/` 系统安装的，系统更新时可能发生覆盖

## 参考

* [How to permanently set $PATH on Linux/Unix [closed]](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix)
* [What is /usr/local/bin?](https://unix.stackexchange.com/questions/4186/what-is-usr-local-bin)