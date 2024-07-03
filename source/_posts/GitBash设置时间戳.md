---
title: GitBash设置时间戳
date: 2024-06-24 09:13:44
tags: 
    - "Git"
categories:
    - "Program"
cover: "/images/git.webp"
---

> 平时工作中，可能会同时处理几件事情，其中有一件要用到 `terminal`，有时候需要在 `terminal` 连续执行多次同个命令，但有时候你地工作被打断，等再回来看 `terminal` 的时候，就不确定工作被打断之前是否执行了命令，因为命令执行没有时间戳
比如下图，完全看不出来什么时候执行的命令行：

## 在命令行执行后面打印

我的方案是在每行命令行执行结束后，打印一下当前的时间戳。具体方法：

### 第一步

用编辑器打开 `git-prompt.sh` 文件，文件目录在 `git` 安装目录下（比如我的是 `D:\APP\Git\etc\profile.d\git-prompt.sh`）。这个文件用于定义打印“命令提示符”。

### 第二步
找到 `PS1="$PS1"'\u@\h ' # user@host<space>` 这行（图中第 20 行），
在这行的上面的第 16 行前面加上时间戳的配置即可。
我加了下面的几行，重点是 `PS1="$PS1"'\t'` 这个，一定要加上，用于获取当前时间，
其他几行是个性化颜色修饰，可有可无。

```sh
PS1="$PS1"'\[\033[45m\]'	# change to Magenta
PS1="$PS1"'>>>'			    # time
PS1="$PS1"' \t'			    # time
PS1="$PS1"'\[\033[0m\]'		# reset style
PS1="$PS1"'\n'			    # new line
```

![配置](/images/git/git1.png)
### 效果图

![效果图](/images/git/git2.png)

## 附录

### ANSI 颜色编码

* ANSI 颜色编码可参考 [ANSI escape code - Wikipedia](https://en.wikipedia.org/wiki/ANSI_escape_code)
* ANSI 控制码
    ANSI 控制码以Esc 作为控制码的开始标志。 Esc的ANS十进制码是 27 ， 八进制码是33， 使用\33 表示。主要的控制码有：

```
\033[0m 关闭所有属性
\033[1m 设置高亮度
\033[4m 下划线
\033[5m 闪烁 ， 测试在在linux 显示效果是底色。
\033[7m 反显
\033[8m 消隐
\033[nA 光标上移n行
\033[nB 光标下移n行
\033[nC 光标右移n行
\033[nD 光标左移n行
\033[y;xH 设置光标位置
\033[2J 清屏
\033[K 清除从光标到行尾的内容
\033[s 保存光标位置
\033[u 恢复光标位置
\033[?25l 隐藏光标
\033[?25h 显示光标
\033[30m ~ \033[37m 设置前景色
\033[40m ~ \033[47m 设置背景色
```
## 参考

* [如何设置 Git Bash 终端命令行执行后打印当前时间](https://zhuanlan.zhihu.com/p/536955408)
* [在终端输出彩色字体](https://blog.csdn.net/weixin_69553582/article/details/125700943)

