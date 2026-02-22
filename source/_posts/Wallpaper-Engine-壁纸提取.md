---
title: Wallpaper Engine 壁纸提取
date: 2025-11-26 13:44:36
tags: 
    - "Wallpaper Engine"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/wallpaper/wallpaper-engine.webp"
excerpt: "壁纸提取"
---

## 提取步骤

### 下载提取工具

访问GitHub上的项目[notscuffed/repkg](https://github.com/notscuffed/repkg),下载最新版本的压缩包(zip文件)并解压缩

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/wallpaper/Snipaste_2025-11-26_13-36-33.png)

### 找到壁纸文件

在Wallpaper Engine中,选择你想提取的壁纸,右键点击并选择"在资源管理器中打开"这将打开包含该壁纸所有文件的文件夹

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/wallpaper/Snipaste_2025-11-26_13-37-52.png)

### 复制文件

找到名为scene.pkg的文件,将其复制到提取工具的文件夹中

### 运行提取命令

在提取工具的文件夹中,打开命令行窗口(在文件夹中按住Shift键并右键点击空白处,选择"在此处打开命令窗口"),输入以下命令：

```bash
.\RePKG.exe extract .\scene.pkg
```

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/wallpaper/Snipaste_2025-11-26_13-39-09.png)

这将生成一个名为output的新文件夹,提取出的图像文件将位于该文件夹中的materials子文件夹中

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/wallpaper/Snipaste_2025-11-26_13-40-39.png)
