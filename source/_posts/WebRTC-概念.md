---
title: WebRTC 概念
date: 2026-01-09 13:27:33
tags:
  - WebRTC
  - Peer-to-Peer
  - 视频
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtcpg.jpg"
---
# WebRTC

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtc7.png)

## 一、前言

WebRTC 技术已经广泛在各个行业及场景中被应用，但对多数开发者来说，实时音视频及相关技术却是比较不常接触到的。

做为一名 Web 开发者，WebRTC 这块的概念着实花了不少时间才搞明白，一是 WebRTC 本身有较多的独有概念，二是虽然带“Web”字样，但依赖底层概念和网络却是 Web 开发很少接触到的；

本篇文章以 0 经验音视频开发者 视角，类比常用的 Web 技术，期望帮助您简单入门 WebRTC 技术，耐心看完本篇文章，你将：

1. 了解什么是 WebRTC
2. 掌握 WebRTC 通话原理
3. 利用 Chrome debug WebRTC 应用

适合阅读对象：Web开发，有 js 基础，对 WebRTC 感兴趣的同学

## 二、使用示例

在进入正文之前，让我们先对它有个基本的印象吧！

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtc0.png)

## 三、简单介绍

有必要再了解一下技术的发展历史、应用场景等，这些能让我们知道它为什么优秀，哪方面优秀，有哪些缺点等。

程序员经常用到 5W1H 分析法，那么本文就按照这个思路给大家做一下介绍：

### What

WebRTC（Web Real-Time Communication），一个可以让用户用自己流量 实现音视频实时通信的框架（APIs），支持浏览器（Firefox、Chrome、safari）以及 iOS、Android 原生系统。

### When

2017 年 12 月成为 W3C 草案，国内微信浏览器 19 年下半年才支持，国内手机自带浏览器目前还有不少兼容问题，2021 年 1 月 26 日，成为 W3C 正式标准。

### Who

2011年 Google 收购多个子项目（GIPS，On2，VPx），成立了现在的 WebRTC 项目，目前是 Google 的一个开源项目。

### Where

可应用在社交/娱乐/教育/工具 等需要实时音视频高效沟通的场景，例如：最近很火的元宇宙。

### Why

W3C 标准，开源，插件化，整体效果佳。

### How

也是本文重中之中，最终的目的也是让大家能知道如何使用。

在正式代码讲解之前，有一些概念需要先普及一下

**MediaStream：** 流媒体对象，音/视频数据的一种封装格式，挂载到 video 或 audio 标签上播放；
**RTCPeerConnection：** 会话控制，网络和媒体信息收发，作用类似 http 对象；
**SDP ：** 主要用于两个会话实体之间的媒体协商，作用类似 http 中的配置项。

结合下图类比会更容易理解：

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtc2.png)

## 四、前置思考问题

在讲解代码前，还需要思考以下几个问题，否则会不清楚为什么代码中需要交换 SDP，cadidate等（您也可以先看完代码后，再回来看这个段落，加深理解）。

双方使用浏览器通信，浏览器能力，网络情况等不一致会对通信有很大影响，一起思考下下面 2 个问题：

### 1、视频编码能力不一样？

peer-A 和 peer-B 是视频互动的两边浏览器，他们通讯前必须在视频编码能力上先达成一致，如下图，最终协商出共同的H264，如果无法达成一致，则通讯失败。

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtc3.png)

### 2 电脑之间，大多数是在某个局域网中，需要 NAT（Network Address Translation，网络地址转换），因此并不能直接通信

显示情况如下图：

![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtc4.png)

通俗一点比喻： 阿宅今年 30 了（不是我，不要乱猜）被父母逼婚，他只能求助媒婆，才可能被另一个阿宅认识。

媒婆解决阿宅社恐问题，NAT 也需要一种方式绕过，双方才能建立通信，我们需要用到 STUN 和 TURN。

## 五、代码讲解

终于到我们的代码讲解部分了，下面的代码会按照推流段顺序，分阶段讲解每个步骤所需要用到的API（如果你是直接看代码，建议看完后再回去看第三、四 Part 的介绍，理解会更加深刻）。

### 步骤一：创建数据源

localStream 作为发送端本地预览画面：

```js
// 创建数据源
const localStream = await navigator.mediaDevices.getUserMedia({
video: true,
audio: true,
});
// 显示数据源，localVideo 是 html 中的 video 标签
localVideo.srcObject = localStream;
```

### 步骤二：创建发送数据实例

```js
// 本地实例
const pc1 = new RTCPeerConnection();
// 对端实例
const pc2 = new RTCPeerConnection();
```

### 步骤三：配置实例

做这一步的目的是为了交换两端的信息**：icecandidate** 和 SDP

1. icecandidate：包含通信协议(TCP/UDP)和通信IP，STUN和TURN协议中描述网络信息的格式规范，解决双方网络链接问题；
1. SDP：浏览器能力，包括不限于音视频编码格式，带宽，流控策略等；解决前置思考中，双方能力不匹配问题，通过交换双方 SDP 浏览器会自动选择双方都支持的视频编码格式。

```js
// 告诉对端，本端地址
pc1.addEventListener('icecandidate', async (e) => {
// 发送给对端
// 对端添加本端地址
if (e.candidate) {
await pc2.addIceCandidate(e.candidate);
}
});


pc2.addEventListener('icecandidate', async (e) => {
// 发送给本端
// 本端添加对端地址
if (e.candidate) {
await pc1.addIceCandidate(e.candidate);
}
});


// 创建本端SDP,告诉本端浏览器支持哪些能力
const offer = await pc1.createOffer();
pc1.setLocalDescription(offer);
// 创建远端SDP,告诉远端浏览器支持哪些能力
const answer = await pc2.createAnswer();
pc2.setLocalDescription(answer);
// 。。。。发送远端SDP给本端
// 接收远端sdp,告诉远端浏览器支持哪些能力
pc1.setRemoteDescription(answer);
// 接收客户端sdp,告诉远端浏览器支持哪些能力
pc2.setRemoteDescription(offer);
```

### 步骤四：发送数据

```js
localStream.getTracks().forEach(
(track) => pc1.addTrack(track, localStream)
);
```

### 步骤五：完整精简版Typescript代码

注意，这里使用的 typescript 编写，实际运行需要先转成 js。

```js
const pc1 = new RTCPeerConnection();
pc1.addEventListener('icecandidate', async (e) => {
if (e.candidate) {
await pc2.addIceCandidate(e.candidate);
}
});
pc1.addEventListener('iceconnectionstatechange', (e) => {
console.log('pc1: iceconnectionstatechange', e);
});


const pc2 = new RTCPeerConnection();
pc2.addEventListener('icecandidate', async (e) => {
if (e.candidate) {
await pc1.addIceCandidate(e.candidate);
}
});


pc2.addEventListener('iceconnectionstatechange', (e) => {
console.log('pc2: iceconnectionstatechange', e);
});


pc2.addEventListener('track', (e) => {
if (e.streams.length > 0) {
remoteVideo.srcObject = e.streams[0];
}
});


const remoteVideo = document.querySelector('#remoteVideo') as HTMLVideoElement;
const localVideo = document.querySelector('#localVideo') as HTMLVideoElement;

async function pushStream(answer: RTCSessionDescriptionInit) {
pc1.setRemoteDescription(answer);
}

async function pullStream(offer: RTCSessionDescriptionInit): Promise<void> {
pc2.setRemoteDescription(offer);
const answer = await pc2.createAnswer();
pc2.setLocalDescription(answer);
console.warn('answer', answer);
pushStream(answer);
}


window.onload = async () => {
const localStream = await navigator.mediaDevices.getUserMedia({
video: true,
audio: true,
});


localVideo.srcObject = localStream;
localStream.getTracks().forEach((track) => pc1.addTrack(track, localStream));


const offer = await pc1.createOffer();
pc1.setLocalDescription(offer);
console.warn('pc1 offer', offer);
pullStream(offer);
};
```

### 步骤六：应用举例

学习完理论知识，接下来我们一起再实践下，加深对知识的理解 —— 通过 Chrome浏览器 debug WebRTC应用。学会 debug 既可以加深理解，也是后续写代码必不可少的技能，千万不少跳过这一步哦：

1. 点击打开示例DEMO
2. 另打开一个tab页面，输入: chrome://webrtc-internals/
3. DEMO 中输入相关信息开始直播, 切回到 2 中的 tab 页面，如下图：
    蓝色部分：对应的是代码中SDP的处理过程
    绿色部分：对应的是网络链接情况
    ![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtc5.png)
4. 继续下来，可以看到推流中实时数据变化：
    蓝色部分：拉流实时数据，包括分辨率，码率，丢包率等
    绿色部分：推流实时数据，包括分辨率，码率，丢包率等
    ![](https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/webrtc/webrtc6.png)
(Identifiers for WebRTC’s Statistics API)
更多字段理解，可戳这里进行深入学习： [Identifiers for WebRTC’s Statistics API](https://www.w3.org/TR/webrtc-stats/)

## 七、结尾

随着硬件网络的更新换代，我们经历了由文字->图片->视频 载体变更的过程。随着 5G 的普及，音视频技术融于无形正在成为现实，WebRTC 作为其中最重要框架之一，浏览器的支持成熟度也在快速完善当中

附下作者常用的工具，推荐收藏：

1. [WebRTC samples.](https://webrtc.github.io/samples/) google 官网 demo，包含最新功能
2. [MediaDevices - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices) 具体接口介绍
3. [WebRTC 1.0: Real-Time Communication Between Browsers](https://w3c.github.io/webrtc-pc/#bib-WEBRTC-STATS) W3C标准介绍

--------------------------------

## 入门补充

下面给出一份“理论速览 + 场景落地”的 WebRTC 入门全景图，全部信息来自 2025-2026 年最新资料，方便你 10 分钟内建立知识框架。

------------------------------------------------
1. 它到底是什么
------------------------------------------------
WebRTC（Web Real-Time Communication）= 一套 **由 W3C 与 IETF 联合制定的开放标准** + **Google 开源的音视频引擎**，目标只有一句话：  
**“让浏览器或移动端不装插件就能实现低延迟、加密、点对点的音视频/数据互通。”**

------------------------------------------------
2. 解决了哪些老问题
------------------------------------------------
| 痛点 | 传统方案 | WebRTC 解法 |
|---|---|---|
| 需装插件/客户端 | Flash、SIP 软电话 | 浏览器原生 API，零安装 |
| 延迟高 | RTMP 1-3 s | 端到端 < 500 ms，典型 100-200 ms |
| 穿 NAT/防火墙难 | 固定端口+中继 | 内置 ICE+STUN+TURN，自动打洞 |
| 移动端碎片化 | 各厂 SDK 不兼容 | 同一套标准，Android/iOS/Web 共用 |
| 数据裸奔 | 自定义协议常未加密 | 强制 SRTP+DTLS，端到端加密 |

------------------------------------------------
3. 三大核心概念（面试常问）
------------------------------------------------
1. **媒体协商** —— SDP（Session Description Protocol）  
   双方互相“自我介绍”能编什么码、分辨率多少，分 Offer/Answer 两次握手。

2. **网络协商** —— ICE（Interactive Connectivity Establishment）  
   收集所有可访问地址（本地 LAN、NAT 映射后公网、TURN 中继），通过 STUN 打洞，失败就转 TURN 中继。

3. **连接抽象** —— RTCPeerConnection  
   把“编解码、加密、拥塞控制、丢包重传”全部封装，开发者只需调 `addTrack`/`createOffer` 等 6 个 API 就能建链。

------------------------------------------------
4. 适用场景（2026 主流落地）
------------------------------------------------
- 1 对 1 通话：Zoom Web 客户端、Google Meet、微信视频号直播连麦  
- 多人会议：SFU 架构（mediasoup、Janus）支撑 1000 人会议室  
- 云游戏：浏览器远程渲染，把手柄输入通过 DataChannel 上行  
- 物联网：Web 端直接查看家用摄像头，无需装 App  
- 文件/屏幕共享：配合 DataChannel 走 P2P，省 30-50% 服务器带宽  
- 低延时直播：电商直播、在线教育，端到端 200 ms 内，秒杀 RTMP

------------------------------------------------
5. 业界已有“开箱即用”的实现
------------------------------------------------
| 名称 | 类型 | 特点 |
|---|---|---|
| **libwebrtc** | 官方 C++ 库 | Chrome/Edge 内核同款，全功能但体积大 |
| **Pion WebRTC** | Go 语言开源 | 云原生友好，二进制 20 MB，适合写 SFU 边缘节点 |
| **mediasoup** | Node.js SFU | 单进程可扛 500 路 720p，支持 SVC 分层 |
| **Janus** | C 语言 SFU | 插件架构，内置录音、SIP 网关 |
| **Flutter-WebRTC** | 移动端插件 | 一套代码跑 Android/iOS，与 Web 端互通 |
| **GStreamer** | 多媒体框架 | 含 webrtcbin 插件，嵌入式/机顶盒常用 |

------------------------------------------------
一句话总结  
WebRTC = **“浏览器里的 TCP/IP + 编解码器 + 加密机”**，把实时通信的 90% 脏活累活封装好，让你只写业务逻辑就能做出 Zoom、Discord、云游戏、远程医疗等任何“低延迟互动”产品。