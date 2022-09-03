---
title: UbuntuServer2204分区
date: 2022-09-03 13:45:07
updated: 2022-09-03 19:52:06
tags:
- linux
---

> 不只适用22.04，其他版本也差不多

分区前的步骤就不多赘述了，可以参考这里的安装配置 [在hyper-v虚拟机中安装并配置linux](https://lovexy.fun/2022/08/30/%E5%9C%A8hyper-v%E8%99%9A%E6%8B%9F%E6%9C%BA%E4%B8%AD%E5%AE%89%E8%A3%85%E5%B9%B6%E9%85%8D%E7%BD%AElinux/)

到分区选择时，选择custom

![](img/UbuntuServer2204分区/1.png)

---

进入后可以看到硬盘信息：

![](img/UbuntuServer2204分区/2.png)

---

选中free space创建一个2G大小的分区挂载到`/boot`，创建后会自动生成一个ESP分区挂载到`/boot/efi`

![](img/UbuntuServer2204分区/3.png)

![](img/UbuntuServer2204分区/4.png)

![](img/UbuntuServer2204分区/5.png)

---

剩下的空间全部创建为`Leave unformatted`的分区

![](img/UbuntuServer2204分区/6.png)

![](img/UbuntuServer2204分区/7.png)

---

创建后我们能看到一个partition 3是unused状态，并且上面`Create volume group (LVM)`也不是灰色了。

然后选中`Create volume group (LVM)`把所有空间都创建成LVM分区。

![](img/UbuntuServer2204分区/8.png)

![](img/UbuntuServer2204分区/9.png)

---

创建完lvm分区后，可以看到硬盘的分区信息变了。

![](img/UbuntuServer2204分区/10.png)

紧接选中`free space`把这些空间创为逻辑卷，依据自己的需求分配大小并挂载到不同位置。

![](img/UbuntuServer2204分区/11.png)

---

最后确认就可以了。

![](img/UbuntuServer2204分区/12.png)
