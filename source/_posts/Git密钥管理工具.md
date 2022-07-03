---
title: Git密钥管理工具
date: 2022-06-30 15:23:51
updated: 2022-06-30 15:23:51
tags:
  - 软件
---

# Git密钥管理工具

## 问题的出现

当你有多个github帐号并且你喜欢使用密钥认证方式的时候会遇到不同帐号密钥更换的问题。 githun的机制是一个rsa公钥只能被绑定一次，无法做到一对rsa密钥在两个以上的账户使用，只能在使用某个帐号时进行手动切换。
每次更改配置文件是一件非常麻烦的事情，为了更优雅的切换密钥而编写了skm。

## 介绍

用Go语言编写，使用cobra用来处理命令行参数，使用promptui简化命令行的输入↑↓键选择要切换的密钥即可。

项目地址：[skm](https://github.com/lovexy-fun/skm)

## 使用说明

可以直接在 [release](https://github.com/lovexy-fun/skm/releases) 页面下载对应操作系统的版本，如果您熟悉Go语言也可以使用源码自行构建。

### skm

使用`skm`命令可以查看当前正在使用的key

输入

```shell
skm
```

输出

```shell
Effective key: testkey
```

### 帮助

输入

```shell
skm -h
```

输出

```shell
SSH key manager

Usage:
  skm [flags]
  skm [command]

Available Commands:
  add         Add a key to manager
  del         Delete a key from manager
  help        Help about any command
  ls          List all keys
  sel         Choose a key to make it effective

Flags:
  -h, --help   help for skm

Use "skm [command] --help" for more information about a command.
```

### 添加

向管理器添加一个私钥

例如:

```shell
skm add -f ./id_rsa -n testkey
```

### 删除

从管理器里删除一个私钥

例如:

```shell
skm del
#然后按↑/↓选择要删除的私钥
```

### 列出所有

列出所有私钥

例如:

```shell
skm ls
```

### 选择

选择一个私钥使其生效

例如:

```shell
skm sel
#然后按↑/↓选择要删除的私钥
```

## Bug反馈&意见建议

[Issues](https://github.com/lovexy-fun/skm/issues)
