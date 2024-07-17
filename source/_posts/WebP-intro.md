---
title: WebP intro
date: 2024-07-17 15:34:17
tags: Image
categories:
    - "Program"
    - "Tools"
    - "Image"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/tools/webp.jpg"
excerpt: "WebP 简介"
---

WebP 是一种现代图片格式，可为网络上的图片提供出色的无损和有损压缩。使用 WebP，网站站长和 Web 开发者可以创建更小、更丰富的图片，从而提高网页加载速度。

与 PNG 相比，WebP 无损图片的尺寸**缩小了 26%**。WebP 有损图片比采用等效 **SSIM** 质量指数的同等 JPEG 图片**小 25-34%**。

无损 WebP 支持透明度（也称为 Alpha 通道），但只需**额外增加 22% 的字节**。对于可以接受有损 RGB 压缩的情况，有损 WebP 也支持透明度，其文件大小通常比 PNG 小 3 倍。

动画 WebP 图片支持有损、无损和透明度，与 GIF 和 APNG 相比，此类图片可缩减大小。

* [面向网站站长的更多信息](https://developers.google.com/speed/webp/faq?hl=zh-cn#how_can_i_detect_browser_support_for_webp)

## WebP 的关键特点

1. 高效的图像压缩
    * 有损压缩：WebP 的有损压缩使用现代的压缩算法，通常可以比 JPEG 更小的文件尺寸来保持类似的图像质量。
    * 无损压缩：WebP 的无损压缩算法通常比 PNG 更高效，能够在不损失图像质量的情况下减少文件大小。
    * 动图支持：WebP 可以用于创建动画图像，类似于 GIF，但文件大小通常比 GIF 小得多。
2. 高质量的图像
    * 支持透明度：WebP 支持 alpha 通道，允许图像具有透明区域，类似于 PNG。
    * 丰富的颜色支持：WebP 支持 24 位 RGB 颜色和 8 位 alpha 通道。
3. 现代的图像格式
    * 支持现代技术：WebP 支持先进的图像压缩技术，如预测编码和变换编码。
    * 较小的文件大小：通常情况下，WebP 文件比 JPEG、PNG 和 GIF 小 30% 左右。

## WebP 与其他图像格式的对比

| 特性         | WebP          | JPEG          | PNG           | GIF           |
|--------------|---------------|----------------|---------------|---------------|
| **压缩类型**   | 有损 / 无损    | 有损           | 无损          | 有损 / 动画   |
| **文件大小**   | 较小           | 较大           | 较大           | 较大 / 动画大 |
| **透明度**     | 支持           | 不支持         | 支持           | 不支持         |
| **动画**       | 支持           | 不支持         | 不支持         | 支持           |
| **质量**       | 高             | 高             | 高             | 低             |

## WebP 的应用场景

* **网页优化**：WebP 的高压缩率使其在网页设计中非常有用，可以减少页面加载时间，提高用户体验。
* **移动应用**：在移动应用中使用 WebP 可以减少数据传输量，优化应用性能。
* **动态图像**：WebP 的动画功能使其成为 GIF 的一个高效替代品。

## WebP 的工具和库

### **工具**

* **[cwebp](https://developers.google.com/speed/webp/docs/cwebp)**：将图像转换为 WebP 格式的命令行工具。
* **[dwebp](https://developers.google.com/speed/webp/docs/dwebp)**：将 WebP 图像转换为其他格式的命令行工具。
* **[WebPShop](https://developers.google.com/speed/webp/download)**：适用于 Photoshop 的 WebP 插件。

### **库**

* **[WebP 官方库](https://github.com/webmproject/libwebp)**：支持 WebP 图像格式的开源库，包含编码和解码功能。
* **[ImageMagick](https://imagemagick.org/)**：支持 WebP 格式的图像处理库。

## WebP 的工作原理

有损 WebP 压缩使用预测编码对图片进行编码，这与 VP8 视频编解码器压缩视频中的关键帧的方法相同。预测编码使用相邻像素块中的值来预测块中的值，然后仅对差值进行编码。

无损 WebP 压缩使用已发现的图像片段来精确重建新像素。如果没有找到有趣的匹配项，它还可以使用本地调色板。

* [WebP 压缩技术详情](https://developers.google.com/speed/webp/docs/compression?hl=zh-cn)

WebP 文件由 VP8 或 VP8L 图片数据以及一个基于 RIFF 的容器组成。独立的 libwebp 库可用作 WebP 规范的参考实现，可以从我们的 Git 代码库或 tarball 获取。

### WebP 格式的技术细节

#### **1. 有损压缩**

WebP 的有损压缩基于 VP8 视频编解码器中的技术：

* **预测编码**：使用邻近像素的颜色信息来预测当前像素的颜色。
* **变换编码**：对像素数据进行离散余弦变换（DCT），以降低数据冗余。

#### **2. 无损压缩**

WebP 的无损压缩技术基于 WebM 项目中的技术：

* **字典压缩**：使用哈夫曼编码和算术编码技术来优化压缩效率。
* **透明度处理**：支持 8 位的 alpha 通道来处理图像的透明部分。

#### **3. 动画功能**

WebP 的动画功能：

* **时间控制**：支持设置帧的显示时间，以创建动画效果。
* **压缩优化**：使用现代算法减少动画帧的重复数据。

## WebP 支持

`Google Chrome` 、`Safari` 、`Firefox` 、`Edge` 、`Opera` 浏览器以及许多其他工具和软件库就原生支持 WebP。开发者还增加了对各种图片编辑工具的支持。

WebP 包含轻量级编码和解码库 libwebp、用于将图片与 WebP 格式相互转换的命令行工具 cwebp 和 dwebp，以及用于查看 WebP 图片、对图片进行多路复用并为其添加动画效果的工具。完整的源代码可在下载页面找到。

| 浏览器         | 支持情况         |
|----------------|------------------|
| **Google Chrome**  | 完全支持          |
| **Mozilla Firefox** | 完全支持          |
| **Safari**        | 从 macOS 11 和 iOS 14 起支持 |
| **Microsoft Edge** | 完全支持          |
| **Opera**        | 完全支持          |
| **Internet Explorer** | 不支持          |

## WebP 转换器下载

下载适用于 [Linux、Windows 或 macOS](https://developers.google.com/speed/webp/docs/precompiled?hl=zh-cn) 的预编译 cwebp 转换工具，将您喜爱的图片集从 PNG 和 JPEG 格式转换为 WebP 格式。

## 参考

* [一种适用于网络的图片格式](https://developers.google.com/speed/webp?hl=zh-cn)
* [WebP 官方文档](https://developers.google.com/speed/webp)
* [WebP 技术详解](https://developers.google.com/speed/webp/docs/tech)
* [WebP 插件下载](https://developers.google.com/speed/webp/download)
