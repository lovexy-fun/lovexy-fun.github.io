---
title: 在hyper-v虚拟机中安装并配置linux
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

### 1、开启Hyper-V功能

在这里默认为你觉得WSL2有问题才考虑使用虚拟机的，那么你的设备一定开启了Hyper-V功能。

另外的，如果你没有使用过WSL2或者电脑系统为Windows10家庭版（Windows11家庭版），那么你需要参考下面的操作（具体操作方式请自行搜索，下面几步并非全部都是完成）。

1. 如果你的系统为家庭版，你可以搜索家庭版开启Hyper-V的教程。 
2. 如果你的系统为家庭版，你可以更换系统为专业版或者使用 [蓝点网](https://www.landiannews.com/) 的系统版本转换工具进行转换（不论是更换系统还是进行转换，都会失去原来的正版系统）。
3. 如果你的系统是专业版，仅仅需要在`控制面板>程序>启用或关闭Windows功能`中打开Hyper-V功能就可以。

### 2、Ubuntu镜像下载

[Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)

写这篇博客时的，最新server lts版本是22.04，目前没有测试18.04。

### 3、创建虚拟机配置（暂时不安装操作系统）

经过前几步的配置后应该可以在Win菜单中搜索到**Hyper-V 管理器**，打开后界面如下：

![](img/在hyper-v虚拟机中安装并配置linux/1.png)

## 最终解决方案

[Buy a Mac](https://www.apple.com/mac/)
