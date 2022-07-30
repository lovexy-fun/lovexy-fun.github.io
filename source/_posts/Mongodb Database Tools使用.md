---
title: Mongodb Database Tools使用
date: 2022-07-24 23:44:12
updated: 2022-07-24 23:44:17
tags:
- MongoDB
---

# Mongodb Database Tools使用

下载地址：[Mongodb Database Tools](https://www.mongodb.com/try/download/database-tools)

## mongodump

使用这个可以导出数据原始的二进制数据

一般可以使用下面的语句进行某个集合数据的导出，还可以加入查询条件导出指定的数据。
```shell
mongodump -h 127.0.0.1:27017 -d testdb -c user_collection -o ./outdate --gzip -q '{\"name\":\"zhangsan\"}'
```

## mongorestore

使用`mongodump`导出的数据需要使用`mongorestore`恢复到指定的数据库的集合里
```shell
mongorestore -h 127.0.0.1:27017 -d testdb -c user_collection --dir ./outdate/testdb/user_collection.bson.gz --gzip
```
