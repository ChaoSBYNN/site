---
title: Python 脚本 进度条显示
date: 2024-07-10 08:28:10
tags: 
    - "Python"
    - "脚本"
categories:
    - "Program"
    - "Python"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/python.png"
excerpt: "文件传输进度条"
---
在 Python 脚本中加入进度条显示通常可以通过第三方库来实现。具体到文件传输的场景，可以使用 `tqdm` 库来显示进度条，无论是使用 Paramiko 还是 SCP 命令都可以适用。下面分别介绍如何在这两种情况下添加进度条显示。

### 使用 `tqdm` 显示进度条

首先，确保已经安装 `tqdm` 库：

```bash
pip install tqdm
```

#### 1. 使用 Paramiko 实现文件传输并显示进度条

```python
import paramiko
from tqdm import tqdm
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

# 获取本地文件大小
local_file_size = os.path.getsize(local_path)

# 建立 SSH 连接
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(hostname, port, username, password)

# 使用 SFTP 协议传输文件
sftp = ssh.open_sftp()

# 显示进度条
with tqdm(total=local_file_size, unit='B', unit_scale=True, desc=f'Uploading {os.path.basename(local_path)}') as pbar:
    def callback(data_transferred, total_size):
        pbar.update(data_transferred - pbar.n)

    sftp.put(local_path, remote_path, callback=callback)

sftp.close()
ssh.close()

print(f"File {os.path.basename(local_path)} transferred successfully to {hostname}:{remote_path}")
```

- `tqdm(total=local_file_size, unit='B', unit_scale=True, desc=f'Uploading {os.path.basename(local_path)}')`：设置进度条，`total` 参数设置为文件大小，`desc` 参数设置进度条描述。
- `callback` 函数用于在传输过程中更新进度条。

#### 2. 使用 SCP 命令和 `subprocess` 实现文件传输并显示进度条

```python
import subprocess
from tqdm import tqdm
import os

# 本地文件路径
local_path = r'C:\path\to\your\file.txt'

# 远程目标路径
remote_path = 'your_linux_username@your_linux_server_ip:/home/your_username/file.txt'

# 获取本地文件大小
local_file_size = os.path.getsize(local_path)

# SCP 命令
scp_command = f'scp "{local_path}" {remote_path}'

# 显示进度条
with tqdm(total=local_file_size, unit='B', unit_scale=True, desc=f'Copying {os.path.basename(local_path)}') as pbar:
    def update_progress(chunk):
        pbar.update(chunk)

    try:
        process = subprocess.Popen(scp_command, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
        while True:
            output = process.stderr.read(1024)
            if output:
                update_progress(len(output))
            else:
                break
        process.wait()
    except Exception as e:
        print(f"Error occurred: {e}")

print(f"File {local_path} transferred successfully to {remote_path}")
```

- `tqdm(total=local_file_size, unit='B', unit_scale=True, desc=f'Copying {os.path.basename(local_path)}')`：设置进度条，`total` 参数设置为文件大小，`desc` 参数设置进度条描述。
- `update_progress` 函数用于在传输过程中更新进度条。

### 注意事项

- 在使用 Paramiko 或者 SCP 命令传输文件时，进度条的更新通常依赖于回调函数或者实时读取输出流的长度。
- `tqdm` 库提供了丰富的配置选项，可以根据需求进行进一步定制，比如调整单位、显示格式等。
- 确保在传输大文件时，进度条的更新是基于传输数据量而不是文件读写操作的频率，以确保进度条的准确性和平滑显示。