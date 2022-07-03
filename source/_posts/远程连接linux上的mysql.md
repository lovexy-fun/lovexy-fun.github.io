---
title: 远程连接linux上的mysql
date: 2020-12-17 11:50:00
updated: 2020-12-17 11:50:00
tags:
  - 踩坑
---

linux服务器安装mysql之后需要进行一些配置操作才能够远程连接

1. 修改**host**、**password**和**plugin**字段

   mysql5.7的user表是没有password的，变成了authentication_string字段。

   plugin需要改为mysql_native_password才能使用密码进行连接。

   ```sql
   update user set host='%',authentication_string=password('要修改的密码'),plugin='mysql_native_password' where user='root';
   ```

2. 修改配置文件中的**bind-address**

   将配置文件中的`bind-address=127.0.0.1`改为`bind-address=0.0.0.0`
