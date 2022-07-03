---
title: Ubuntu创建应用快捷方式
date: 2018-11-01 23:24:00
updated: 2018-11-01 23:24:00
tags:
  - linux
  - 瞎搞
---

# Ubuntu创建应用快捷方式

新建一个.desktop文件

```shell
vi eclipse.desktop
```

然后又进行编辑

```ini
[Desktop Entry]
Encoding=UTF-8
Name=eclipse
GenericName=eclipse IDE
Comment=java ide
Exec=/etc/eclipse/eclipse
Icon=/etc/eclipse/icon.xpm
Terminal=false
Type=Application
Categories=Application;Development;
```

| 关键字          | 含义                   | 参数            |
| --------------- | ---------------------- | --------------- |
| [Desktop Entry] | 标识                   |                 |
| Encoding        | 编码                   | UTF-8等编码格式 |
| GenericName     | 描述                   | 一句描述        |
| Comment         | 注释                   | 一句注释        |
| Exec            | 可执行文件目录及文件名 | 文件名及目录    |
| Icon            | 图标目录及图标名       | 文件名及目录    |
| Terminal        | 是否启动终端           | true/false      |
| Type            | 启动器类型             |                 |
| Categories      | 应用类型               |                 |

编写完后保存退出，然后执行

```shell
chmod +x eclipse.desktop
```

如果想让这个快捷方式在全部应用里能找到，就把这个文件拷贝到**/usr/share/application/**下
