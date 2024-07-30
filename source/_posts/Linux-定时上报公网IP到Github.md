---
title: Linux 定时上报公网IP到Github
date: 2024-07-30 08:39:45
tags: 
    - "Linux"
    - "Tools"
categories:
    - "Program"
    - "Linux"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/linux.png"
excerpt: "Linux 定时任务, 系统变量使用, IP获取"
---

## 获取公网IP

```bash
curl icanhazip.com
```

## 在Linux中，你可以使用date命令获取当前日期和时间。以下是一些常用的date命令用法示例:

获取当前日期和时间:

```bash
date
```

获取当前日期:

```bash
date +%Y-%m-%d
```

获取当前时间:

```bash
date +%H:%M:%S
```

获取当前日期和时间，并自定义格式:

```bash
date +"%Y-%m-%d %H:%M:%S"
```

这里的 `+%Y-%m-%d` , `+%H:%M:%S`  是格式化字符串，分别代表年-月-日、时:分:秒。你可以根据需要自定义这些格式。

## 打印公网IP + 日期时间

```bash
echo $(curl icanhazip.com) - $(date) > ip.yaml
```

## git commit 动态加入日期

```bash
git commit -m "Your commit message $(date +'%Y-%m-%d')"
```

## git 提交

```bash
vim report.sh
---------------------------------------
echo $(curl icanhazip.com) - $(date) > /path/to/ip.yaml
git add .
git commit -m "feat: $(date '+%Y-%m-%d')"
git push origin
```

## 在Linux中，可以使用cron来设置定时任务执行.sh脚本。以下是设置定时任务的步骤

打开终端。

输入crontab -e命令编辑当前用户的cron任务。

在打开的编辑器中，添加定时任务的行。

保存并退出编辑器。

例如，如果你想每天早上5点执行名为script.sh的脚本，你可以这样设置:

```bash
0 5 * * * /path/to/script.sh
```

这行的意思是:分钟(0) 小时(5) 每天的哪个小时() 每月的哪一天() 每年的哪一月(*)

确保你的.sh脚本有执行权限:

```bash
chmod +x /path/to/script.sh
```

如果你的script.sh在当前用户的home目录下，并且已经有执行权限，则可以直接使用相对路径:

```bash
0 5 * * * ./script.sh
```

注意:如果脚本中有相对路径，请确保这些相对路径是相对于当前工作目录(不是脚本文件本身的位置)。

## 设置定时任务

```bash
crontab -l
```

## 要查看当前用户的 crontab(定时任务)

要查看当前用户的 crontab(定时任务)，可以使用以下命令:

```bash
crontab -l
```

这条命令会列出当前用户的所有 crontab 任务。如果 crontab 为空，则不会显示任何内容。

如果你想查看其他用户的 crontab(例如 root 用户)，你可以使用以下命令:

```bash
sudo crontab -l -u username
```

其中 `username` 是你要查看其 crontab 的用户名。使用 `sudo` 是因为只有 root 或者有 sudo 权限的用户才能查看其他用户的 crontab。

## crontab 使用 vim 模式

如果你想在编辑 crontab 文件时使用 Vim 编辑器而不是默认的 Nano 编辑器，可以按照以下步骤操作:

### **设置默认编辑器**

在终端中运行以下命令，选择 Vim 作为默认的 crontab 编辑器:

```bash
export VISUAL=vim
export EDITOR=vim
```

这两条命令将 VISUAL 和 EDITOR 环境变量设置为 vim。你也可以将它们添加到你的 `~/.bashrc` 或 `~/.bash_profile` 文件中，这样每次打开终端时都会自动设置。

### **编辑 crontab 文件**

现在，运行 `crontab -e` 命令会自动使用 Vim 编辑器打开 crontab 文件。你可以像在普通的 Vim 中编辑文件一样进行操作。

### **保存和退出**

在 Vim 编辑器中，按下 `Esc` 键进入命令模式，然后输入 `:wq` 并按 `Enter` 键保存并退出文件。

通过这种方法，你可以使用 Vim 编辑器来编辑 crontab 文件，而不是默认的 Nano 编辑器。
