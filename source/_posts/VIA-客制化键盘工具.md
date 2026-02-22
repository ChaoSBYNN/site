---
title: 'VIA: 客制化键盘工具'
date: 2025-04-25 11:03:55
tags:
    - "VIA"
    - "Keyboard"
    - "Tool"
categories:
    - "VIA"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/via-banner.jpg"
excerpt: "客制化键盘"
---

## Introduction

> VIA 是一款开源的键盘配置工具，全称为 Virtual Keyboard Interface，主要用于客制化键盘的自定义设置。它允许用户通过图形化界面轻松地自定义键盘的按键映射、灯光效果以及其他功能。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_11-23-40.png)

### VIA 的主要功能

1. **按键映射自定义**：用户可以根据自己的使用习惯，将键盘上的任意按键重新映射为其他按键。例如，将“Q”键改为“A”键。
2. **宏定义**：可以设置复杂的按键组合或序列，通过一个按键触发多个操作。比如将某个按键设置为复制（Alt+C）操作。
3. **多层设置**：支持多层按键布局，用户可以在不同层上设置完全不同的按键功能。
4. **灯光效果调整**：对于支持 VIA 的键盘，用户可以自定义键盘的灯光效果，包括颜色、亮度和动态效果。
5. **Any 键功能**：允许用户将任意按键设置为特殊功能键，如“Ctrl”或“Shift”。

### 使用 VIA 的步骤

1. **确认键盘支持**：确保你的客制化键盘支持 VIA 功能，并获取对应的 `.json` 文件。
2. **安装 VIA**：从 GitHub 下载 VIA 软件，或通过网页版直接访问。
3. **导入配置文件**：打开 VIA 软件后，导入键盘对应的 `.json` 文件。
4. **自定义设置**：在 VIA 的图形化界面中，根据需要进行按键映射、宏定义等设置。
5. **保存并应用**：完成设置后，保存配置并应用到键盘上。

### VIA 的优势

- **简单易用**：通过图形化界面，即使是新手也能快速上手。
- **高度自定义**：提供了丰富的自定义选项，满足不同用户的需求。
- **开源免费**：VIA 是一个开源项目，用户可以免费使用。

VIA 为客制化键盘爱好者提供了一个强大的工具，可以轻松实现个性化设置，提升键盘的使用体验。

## 使用

- [VIA 官网](https://usevia.app/)
- [QMK架构VIA](http://doiokb.com/index.php?m=home&c=Lists&a=index&tid=191)

> VIA 本版不统一 需要甄别使用

### 设备授权

#### 链接设备: 依次点击 `Authorize device +` > `keyboard` > `连接`

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_13-23-38.png)

#### 开启设计模式: 依次点击 `SETTINGS` > `Show Design tab` > `ON`

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_13-27-05.png)

#### 导入json文件: 依次点击 `DESIGN` > `Load` > `.json文件`

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_13-27-28.png)

#### 成功识别设备

<div style="display: flex; justify-content: space-between;">
    <img src="https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_11-25-29.png" alt="DOIO 左手" style="width: 90%;"/>
    <img src="https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_11-25-09.png" alt="DOIO 右手" style="width: 90%;"/>
</div>

## 改键

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/1-23112G343591A.jpg)

### 基础键位

左侧 `KEYMAP` 菜单修改键位 点击上方设计键位槽 然后点击下方选择目标键修改

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_11-25-29.png)

### 层 (Layer)

1. 在 `layer0` 层按下 `5` 就输入一个 `5`
2. 在 `layer5` 层按下 `5` 就是调亮灯光 `RGB SPI`

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_14-00-10.png)

> 不同设备层切换方式不同 最常用的是 `MO` 和 `TO`

1. `MO(*)`：该键为组合按键，当按下该键，当前键盘键值即可临时切换为对应层键盘键值，同时按下对应层按键，即可实现对应层键值功能输入。
2. `TO(*)`：该键为层切换按钮，按下即可切换到目标层键盘键值。
3. `OSL(*)`：当按下该键后，键盘键值会切换为目标层键值，点击目标曾某一按键后，即恢复原键盘键值。
4. `TG(*)`：该键为层按键切换按钮，按下即可将当前键盘键值切换为目标键盘键值，再按下即可恢复到原键盘键值。
5. `TT(*)`：
    - 按下即可将当前键盘键值临时切换为对应层键盘键值，同时按下对应层按键，即可实现对应层键值功能输入
    - 连续点击5下，当前键盘键值即可切换至目标层键值。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_14-00-39.png)

> *如果是三模的键盘，务必要保留有线功能的切换，否则再您切换到无线功能之后，将无法切回至有线功能，也就无法使用VIA，只能进行键盘重置才可以切回*

### 功能键

> 除了层级切换功能键意外还包含更多功能键设置

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_14-00-57.png)

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_14-01-10.png)

### 宏

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/via/Snipaste_2025-04-25_14-01-34.png)

> To be continued

## 常见问题

1. 无法识别设备
    - 检查设备是否支持VIA
    - 检查设备是否连接正确
    - 检查设备是否开启设计模式
    - 桌面版无法识别 可以尝试使用网页版链接后正常识别
2. 导入json文件 VIA不能识别键盘的问题
    - 在 `DESIGN` 页面，打开或关闭 `Use V2/3 definitions` 按钮，然后返回 `CONFIGURE` 页面，就可以看到键盘被识别了。
3. 为什么VIA没有any键
    - 有些汉化版via没有any键 使用网页版或者英文原版via

## 参考

- [VIA Doc](https://www.caniusevia.com/docs/specification)
- [VIA 改键基础教程](https://zhuanlan.zhihu.com/p/651406695)
- [客制化键盘VIA改键进阶教程及ANY键初入门](https://zhuanlan.zhihu.com/p/599850387)