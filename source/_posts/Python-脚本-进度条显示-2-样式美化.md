---
title: Python 脚本 进度条显示 2 样式美化
date: 2024-07-11 10:47:43
tags: 
    - "Python"
    - "脚本"
categories:
    - "Program"
    - "Python"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/python.png"
excerpt: "进度条美化"
---

`tqdm` 增加 `bar_format` , `ascii` 配置

### 进度条背景样式

`ascii="-━"` 需两个字符

* 首字符 `-` 为背景
* 尾字符 `━` 为实际进度

![效果](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/process_bar.png)

### python 执行命令日志同一行刷新

* 加入`\r` `desc=f'\rUploading {os.path.basename(local_path)}'`

```python
import paramiko
from tqdm import tqdm
import os

# SSH 连接信息
hostname = 'your_linux_server_ip'
port = 22
username = 'your_linux_username'
password = 'your_linux_password'


# 远程目标路径
remote_path = '/home/target_path/file.txt'
base_path = r'E:\source_path'

# 使用 os.path.join() 拼接路径
local_path = os.path.join(base_path, filename)

# 远程目标路径
remote_path = '/root/' + filename

# 获取本地文件大小
local_file_size = os.path.getsize(local_path)

# 建立 SSH 连接
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(hostname, port, username, password)

# 使用 SFTP 协议传输文件
sftp = ssh.open_sftp()

# 进度条样式
bar_format = "{desc}: {percentage:3.0f}%|{bar:42}| {n_fmt}/{total_fmt} [{elapsed}<{remaining}, {rate_fmt}]"


# 显示进度条
with tqdm(total=local_file_size,
          unit='B',
          unit_scale=True,
          desc=f'\rUploading {os.path.basename(local_path)}',
          bar_format=bar_format,
          ascii="-━",
          ncols=42,
          colour='green',
          dynamic_ncols=True) as pbar:
    def callback(data_transferred, total_size):
        pbar.update(data_transferred - pbar.n)

    sftp.put(local_path, remote_path, callback=callback)
sftp.close()

# 关闭 SSH 连接
ssh.close()

print(f"File {os.path.basename(local_path)} transferred successfully to {hostname}:{remote_path}")

```