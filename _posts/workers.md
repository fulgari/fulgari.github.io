---
title: "搞清楚 Worker 家族"
date: "2022-11-10"
---

## 分类

![workers.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b4f8af631484d82bd04f73316480397~tplv-k3u1fbpfcp-watermark.image?)

## Web Workers

### 背景

JS 单线程限制，执行大量计算时会阻塞渲染。于是通过 Web Worker 来减轻主线程中*计算密集型*工作。

### 原理

同在主浏览器进程下，存在 JS 主线程，而一旦创建了 Web Worker，它就会在 JS 主线程之外创建的 Worker 线程中执行相应的脚本代码。

### 特点

- Worker 和 JS 主线程之间传递数据：发送端使用 `postMessage()` 发送数据，接收端使用事件监听器 `onmessage` 监听
- 每个 Worker 线程中都有一个独立的全局环境，该环境不能够访问到 DOM、BOM，只能基于事件触发来传输数据

### 应用

典型场景：通常用于*计算密集型*操作，或者*在一个不可预知的时间访问到数据*。
- 预加载并缓存数据供以后使用
- 轮询和处理来自服务端的数据
- 大数据集的计算和处理
- 游戏中与移动相关的计算
- 图像、视频、音频的处理和过滤
- 处理文本数据（代码语法、拼写检查、字数）

## Service Worker

### 背景

sw 最大的意义就是提供了*离线打开网页*的功能。如果需要实现 Offline App，浏览器和服务器之间需要有一个*服务器代理*网络请求，这个服务器代理就叫 service worker。即使不是离线情况下使用，也能够优先使用缓存内容，减少对服务器请求。

### 原理

- 基于 web worker（一个独立于 JS 主线程的独立线程，在里面执行需要消耗大量资源的操作不会阻塞主线程）
- 在 web worker 的基础上增加了离线缓存的能力
- 本质上充当 Web 应用程序服务器与浏览器之间的代理服务器（可以拦截全站的请求，并作出相应的动作->由开发者指定的动作）

### 特点

- 创建有效的离线体验（将一些不常更新的内容缓存在浏览器，提高访问体验）
- 完全异步，由事件驱动，具有生命周期
- 可以访问 cache 和 indexDB
- 支持推送
- 并且可以让开发者自己控制缓存的内容和版本

### 注意事项

- 出于安全性考虑，只有 https 可以承载 serviceWorker
- 其生命周期完全独立于页面