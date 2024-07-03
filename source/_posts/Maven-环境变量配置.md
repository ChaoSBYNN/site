---
title: Maven 环境变量配置
date: 2024-06-24 08:56:13
tags: 
    - "Env"
    - "Maven"
    - "Java"
categories:
    - "Program"
cover: "/images/maven.jpg"
---

`Maven`的环境变量配置主要涉及到设置`MAVEN_HOME`和更新系统的`PATH`环境变量。以下是在Windows和Linux/macOS系统上配置`Maven`环境变量的步骤：

## Windows系统：

1. 设置`MAVEN_HOME`：
  * 找到`Maven`的安装目录，例如`C:\Program Files\apache-maven-3.8.1`（具体路径根据你的安装情况而定）。
  * 右键点击“我的电脑”或“此电脑”，选择“属性”，然后点击“高级系统设置”。
  * 在系统属性窗口中，点击“环境变量”按钮。
  * 在系统变量区域，点击“新建”，变量名填写`MAVEN_HOME`，变量值填写你的`Maven`安装路径。
2. 更新`PATH`环境变量：
  * 在系统变量区域，找到`Path`变量，选中它然后点击“编辑”。
  * 在值的末尾添加`;%MAVEN_HOME%\bin;`（注意前面的分号）。这确保了系统可以在`Maven`的`bin`目录中找到执行文件。
  * 点击“确定”保存更改。

现在，当你打开命令提示符或PowerShell时，应该能够使用mvn命令来调用Maven。

## Linux/macOS系统：

1. 编辑`bash`配置文件（例如`.bashrc`或`.bash_profile`）：
    打开终端，然后输入以下命令来编辑你的bash配置文件（具体取决于你的系统配置）：
    ```sh
    nano ~/.bashrc  # 或者使用你喜欢的文本编辑器，如 vim, emacs 等。
    ```
2. 设置`MAVEN_HOME`：
    在打开的文件中，添加或修改以下行：
    ```sh
    export MAVEN_HOME=/path/to/apache-maven  # 替换为你的Maven安装路径。
    ```
3. 更新`PATH`环境变量：
    在文件的末尾添加以下行：
    ```sh
    export PATH=$PATH:$MAVEN_HOME/bin  # 这将Maven的bin目录添加到你的PATH中。
    ```
4. 保存并关闭文件：在`nano`中，按`Ctrl + X`，然后按`Y`保存更改，最后按`Enter`退出。
5. 使更改生效：在终端中输入以下命令来使更改立即生效：
    ```sh
    source ~/.bashrc  # 或者使用你的配置文件名。
    ```
现在，你应该能够在终端中使用`mvn`命令来调用`Maven`了。如果你使用的是其他`shell`（如`zsh`），请确保你在正确的配置文件中进行了更改。