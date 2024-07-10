---
title: Python 脚本 Windows to Linux 文件传输
date: 2024-07-10 08:27:51
tags: 
    - "Python"
    - "脚本"
categories:
    - "Program"
    - "Python"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/python.png"
excerpt: "SFTP 跨平台文件传输 "
---
要在 Python 中实现从 Windows 到 Linux 的文件传输，可以使用多种方法，这里介绍两种常见的方式：使用 Paramiko 库进行 SSH 文件传输和使用 SCP 命令的 subprocess 模块。

### 方法一：使用 Paramiko 进行 SSH 文件传输

Paramiko 是一个用于 SSHv2 协议的 Python 实现，可以通过它来建立 SSH 连接，并在 Windows 上执行 Python 脚本来传输文件到 Linux。

1. **安装 Paramiko**

   在 Windows 上安装 Paramiko 库：

   ```bash
   pip install paramiko
   ```

2. **编写 Python 脚本**

   下面是一个简单的示例脚本，演示如何使用 Paramiko 在 Windows 上将文件传输到 Linux 主机：

   ```python
   import paramiko
   import os

   # SSH 连接信息
   hostname = 'your_linux_server_ip'
   port = 22
   username = 'your_linux_username'
   password = 'your_linux_password'

   # 本地文件路径
   local_path = r'C:\path\to\your\file.txt'

   # 远程目标路径
   remote_path = '/home/your_username/file.txt'

   # 建立 SSH 连接
   ssh = paramiko.SSHClient()
   ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
   ssh.connect(hostname, port, username, password)

   # 使用 SFTP 协议传输文件
   sftp = ssh.open_sftp()
   sftp.put(local_path, remote_path)
   sftp.close()

   # 关闭 SSH 连接
   ssh.close()

   print(f"File {os.path.basename(local_path)} transferred successfully to {hostname}:{remote_path}")
   ```

   - 将 `your_linux_server_ip` 替换为你的 Linux 服务器 IP 地址。
   - 将 `your_linux_username` 和 `your_linux_password` 替换为 Linux 服务器的用户名和密码。
   - 将 `local_path` 替换为你要传输的本地文件路径。
   - 将 `remote_path` 替换为 Linux 上的目标路径，用于存储传输的文件。

3. **运行 Python 脚本**

   在 Windows 上运行这个 Python 脚本，它会连接到指定的 Linux 服务器，并将文件传输到远程目标路径。

### 方法二：使用 SCP 命令和 subprocess 模块

另一种常见的方法是使用 Python 的 `subprocess` 模块来执行 SCP 命令，直接调用系统的 SCP 工具进行文件传输。

1. **确保本地 Windows 上有安装 SCP 客户端。**

   通常可以通过安装 Git for Windows 或者单独安装 SCP 客户端来获取 SCP 工具。

2. **编写 Python 脚本**

   ```python
   import subprocess

   # 本地文件路径
   local_path = r'C:\path\to\your\file.txt'

   # 远程目标路径
   remote_path = 'your_linux_username@your_linux_server_ip:/home/your_username/file.txt'

   # SCP 命令
   scp_command = f'scp "{local_path}" {remote_path}'

   # 执行 SCP 命令
   try:
       subprocess.run(scp_command, shell=True, check=True)
       print(f"File {local_path} transferred successfully to {remote_path}")
   except subprocess.CalledProcessError as e:
       print(f"Error occurred: {e}")
   ```

   - 将 `local_path` 替换为你要传输的本地文件路径。
   - 将 `remote_path` 替换为 Linux 上的目标路径，格式为 `username@server_ip:path/to/remote/file.txt`。

3. **运行 Python 脚本**

   在 Windows 上运行这个 Python 脚本，它会调用系统的 SCP 工具，将文件传输到指定的 Linux 服务器。

### 注意事项：

- 确保 Windows 上的网络连接能够访问到 Linux 服务器。
- 在使用 Paramiko 或 SCP 传输文件时，建议确保 Linux 服务器的 SSH 服务正常运行，并且有相应的权限和路径可用于文件传输。
- 根据实际情况，可以根据需要进行进一步的错误处理和安全设置，比如使用 SSH 密钥认证替代密码认证等。