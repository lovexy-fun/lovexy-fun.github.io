---
title: 更优雅地使用linux做开发
date: 2022-08-14 23:40:14
updated: 2022-08-14 23:40:20
tags:
- linux
---

> 系统版本：Ubuntu 20.04 LTS / WSL2 Ubuntu 20.04 LTS
> 内容不定时更新

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
