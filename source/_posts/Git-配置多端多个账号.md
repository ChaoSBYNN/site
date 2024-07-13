---
title: Git 配置多端多个账号
date: 2024-06-26 12:12:15
tags: 
    - "Git"
categories:
    - "Program"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/git.webp"
---

## 清空默认的全局 user.name 和 user.email

```sh
git config --global --unset user.name
git config --global --unset user.email
```

查看git配置

```sh
git config --global --list
```

## 配置多个git的用户名和邮箱

1. 单个全局配置

```sh
git config --global user.name "yourusername"
git config --global user.email "youremail@email.com"
```
2. 多个配置

> 注意: 这里`git config`命令没有带`—global`,表示这是一个局部的设置,也就是这个用户是当前项目的,而不是全局的

```sh
git config user.name "yourusername"
git config user.email "youremail@hotmail.com"
```

3. 删除配置

```sh
git config --unset user.name
git config --unset user.email
```

## 生成多个密钥


1. 生成`gitlab`仓库的SSH

指定文件路径,方便后面操作:`~/.ssh/id_rsa.gitlab`,`id_rsa.github`是秘钥的别名。

```sh
ssh-keygen -t rsa -f ~/.ssh/id_rsa.gitlab -C "gitlab@mail.com"
```

2. 生成`github`仓库的SSH

```sh
ssh-keygen -t rsa -f ~/.ssh/id_rsa.github -C "github@mail.com"
```
3. 将 `ssh-key` 分别添加到 `ssh-agent` 信任列表

```sh
ssh-agent bash
ssh-add ~/.ssh/id_rsa.gitlab
ssh-add ~/.ssh/id_rsa.github
```
如果看到 `Identitiy added: ~/.ssh/id_ras_github` 就表示添加成功了。

4. 添加公钥到自己的 git 账户中

5. 在 config 文件配置多个 ssh-key
在生成密钥的`.ssh` 目录下,新建一个`config`文件,然后配置不同的仓库

```sh
#Default github user Self
Host github.com
    HostName github.com
    User git #默认就是git,可以不写
    IdentityFile ~/.ssh/id_rsa.github

#Add gitlab user 
Host gitlab.com  # 别名,最好别改
    HostName gitlab.com
    User xxx@mail.com #用户名
	#密钥文件的地址,注意是私钥
	IdentityFile ~/.ssh/id_rsa_gitlab
```

6. 测试
```sh
ssh -T git@gitee.com
```