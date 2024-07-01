---
title: webp 转换工具 cwebp
date: 2024-07-01 10:23:00
tags: Image
categories:
    - "Program"
    - "Tools"
    - "Image"
cover: "/images/tools/webp.jpg"
excerpt: "webp图片转换工具"
---

> webp是Google推出的一种新式图片格式、相比于常用的jpg、png和gif格式，最大的优势就是同等质量下压缩率更高、图片文件更小、利于节约存储空间和网络带宽。

## 安装

`https://storage.googleapis.com/downloads.webmproject.org/releases/webp/index.html`

## 配置

下载完成后解压至某个路径下,然后将其中的`bin`目录路径添加至环境变量`path`下即可

## 验证

```sh
cwebp -version
```

```sh
1.4.0
libsharpyuv: 0.4.0
```

## 使用

```sh
cwebp input.png -o output.webp
```

## 效果

```sh
680K -rw-r--r-- 1 Windows 197121 677K  7月  1 09:52  maven.png
 36K -rw-r--r-- 1 Windows 197121  34K  7月  1 10:21  maven.webp
```