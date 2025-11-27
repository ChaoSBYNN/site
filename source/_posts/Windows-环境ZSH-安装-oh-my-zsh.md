---
title: Windows 环境ZSH 安装 oh-my-zsh
date: 2025-11-27 15:26:13
tags:
    - "Terminal"
categories:
    - "Terminal"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/terminal/_20251127144715_105_16.png"
excerpt: "终端美化"
---
## 安装 oh-my-zsh 与常用插件

1. 一条命令装 oh-my-zsh  

   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```

2. 装插件(示例)  

   ```bash
   git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
   ```

   把插件名加到 `~/.zshrc` 的 `plugins=(git zsh-autosuggestions zsh-syntax-highlighting)` 一行,再 `source ~/.zshrc`

## (可选)Powerlevel10k 主题

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

编辑 `~/.zshrc`  

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

重新打开 Git Bash 会进入 p10k 配置向导,按提示选样式即可

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/terminal/Snipaste_2025-11-27_15-39-26.png)