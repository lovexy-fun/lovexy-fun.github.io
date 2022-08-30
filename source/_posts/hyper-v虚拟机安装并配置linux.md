---
title: hyper-v虚拟机安装并配置linux
date: 2022-08-30 17:04:52
updated: 2022-08-30 17:04:58
tags:
- linux
---

## WSL2真香？

WSL2相比于WSL1前者更类似于虚拟机，配合上[Windoes Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=zh-cn&gl=CN)的漂亮界面以及通过localhost直接访问，让人直呼真香!

那么WSL2真香吗？

WSL2理所当然的可以运行docker，你可以把一些mysql、redis之类的服务放在docker容器里运行。

当有一天你会发现在使用`systemctl`命令的时候会报这样的错误：
```shell
System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down
```

出现这个问题是有解决方案的，去google搜索也能找到教程，但是点进教程连接我就默默地离开了页面（完全不想看）。

既然systemctl存在这样的问题，那其他地方也可能存在着问题，不如就直接使用虚拟机了。

## Hyper-V方案

### Ubuntu镜像下载

[Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)

写这篇博客时的，最新server lts版本是22.04，目前没有测试18.04。

### 开启Hyper-V功能


## 不差钱方案

[Buy a Mac](https://www.apple.com/mac/)