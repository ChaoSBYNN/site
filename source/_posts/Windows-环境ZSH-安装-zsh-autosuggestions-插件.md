---
title: Windows 环境ZSH 安装 zsh-autosuggestions 插件
date: 2025-11-27 15:41:34
tags:
    - "Terminal"
categories:
    - "Terminal"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/terminal/_20251127144715_105_16.png"
excerpt: "终端美化"
---

> zsh-autosuggestions 是一个命令提示插件, 自动推测你可能需要输入的命令,按右键可以快速采用建议

### 安装 zsh-autosuggestions 插件

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### 启用插件

编辑 `~/.zshrc` 文件, 找到 `plugins` 行, 添加 `zsh-autosuggestions` 插件

```bash
plugins=(
    git
    zsh-autosuggestions
)
```

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/terminal/Snipaste_2025-11-27_15-46-13.png)
