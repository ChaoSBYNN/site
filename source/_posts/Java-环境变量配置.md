---
title: Java 环境变量配置
date: 2024-06-24 08:39:02
tags: 
    - "Env"
    - "Java"
categories:
    - "Program"
cover: "/images/java.jpg"
---
> 💡 安装后需配置环境变量

Java 环境变量配置主要涉及设置 `JAVA_HOME` 和更新系统的 `PATH` 环境变量。以下是详细的步骤和说明：

## 确定 Java 安装路径

首先，你需要确定 Java 的安装路径。通常，Java 会安装在 `C:\Program Files\Java\` 目录下。你可以通过“控制面板”中的“程序和功能”来查找已安装的 Java 版本，或者通过命令行使用 `where java` 来查找 Java 可执行文件的路径。

## 设置 JAVA_HOME 环境变量

JAVA_HOME 环境变量用于指定 Java 的安装目录。你可以按照以下步骤设置它：

### Windows 系统:

1. 打开“控制面板”。
2. 点击“系统与安全”。
3. 点击“系统”。
4. 点击“高级系统设置”。
5. 在“高级”选项卡下，点击“环境变量”。
6. 在“系统变量”下，点击“新建”，变量名输入 `JAVA_HOME`，变量值填入你的 Java 安装路径（例如 `C:\Program Files\Java\jdk11_XXX`）。

### Linux 或 macOS 系统:

你可以在终端中使用文本编辑器(如 `nano` 或 `vim`)编辑 `.bashrc`,`.bash_profile` 或 `.zshrc` 文件(取决于你的 `shell` 和操作系统)，然后添加以下行：

```sh
export JAVA_HOME=/path/to/java-installation
```

将 `/path/to/java-installation` 替换为你的 Java 安装路径。

## 更新 PATH 环境变量

你需要将 Java 的 `bin` 目录添加到系统的 `PATH` 环境变量中，这样你就可以从任何位置运行 Java 命令。

### 对于 Windows 系统:

在“环境变量”窗口中，找到并选中`Path`变量，点击“编辑”。在编辑窗口中，添加 `JAVA_HOME\bin` 到值的开头或任何合适的位置。确保用分号 ; 分隔不同的路径。

### 对于 Linux 或 macOS 系统:

在 `.bashrc`,`.bash_profile` 或 `.zshrc` 文件中添加以下行：

```sh
export PATH=$PATH:$JAVA_HOME/bin
```

保存文件并重新加载配置(在终端中运行 `source ~/.bashrc` 或相应的配置文件)。

## 验证配置

配置完成后，你可以通过打开命令行或终端，并运行 `java -version` 和 `javac -version` 来验证 Java 是否正确配置。这两个命令分别显示 Java 运行时和编译器的版本信息。
以上步骤应该能帮助你正确配置 Java 的环境变量。如果你在使用过程中遇到任何问题，请随时提问！