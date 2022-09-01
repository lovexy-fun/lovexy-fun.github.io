---
title: 更优雅地使用linux做开发
date: 2022-08-14 23:40:14
updated: 2022-08-14 23:40:20
tags:
- linux
---

> 系统版本：Ubuntu 20.04 LTS / WSL2 Ubuntu 20.04 LTS
> 内容不定时更新

## 系统选择

如果对某讯的软件没需求的话完全可以只使用linux桌面发行版本。

当然也可以使用linux桌面发行版+deepin-wine。

如果忍受不了deepin-wine的兼容性bug，可以选择Windows+WSL2。

如果觉得WSL2不完整或有Bug，那么可以使用Hyper-V虚拟机，教程参考：[在hyper-v虚拟机中安装并配置linux](https://lovexy.fun/2022/08/30/%E5%9C%A8hyper-v%E8%99%9A%E6%8B%9F%E6%9C%BA%E4%B8%AD%E5%AE%89%E8%A3%85%E5%B9%B6%E9%85%8D%E7%BD%AElinux/)

如果觉得Hyper-V性能不够，可以关闭BIOS的虚拟化，使用VMware虚拟机。

如果觉得虚拟机不方便，那么可以换个Mac.

## 更优雅地使用软件

### 添加用户到软件的权限组

有些软件安装完成后会创建一个自己的用户组，有时如果不切换到root去使用软件会报错，这时把自己当前用户添加到软件的用户组就是解决权限问题。比如docker会创建一个docker用户组。

添加用户到组示例如下：
```shell
cat /etc/group #查看所有的用户group
usermod -a -G OutlawManiac ZhangSan #添加用户ZhangSan到OutlawManiac用户组
```

### Desktop版本系统创建桌面图标

参考：[Ubuntu创建应用快捷方式](https://lovexy.fun/2018/11/01/Ubuntu%E5%88%9B%E5%BB%BA%E5%BA%94%E7%94%A8%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F/)

### 某些环境使用docker来搭建

有时一些环境只是临时开发需要，如果你并不需要长期装在实体机上可以考虑使用docker容器

一些环境的搭建请参考：[用docker搭建开发环境](https://lovexy.fun/2022/08/16/%E7%94%A8docker%E6%90%AD%E5%BB%BA%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/)
