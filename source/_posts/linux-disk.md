---
title: linux磁盘管理
date: 2025-03-23 13:39:57
cover: /img/linux-disk/3.png
categories: linux
tags:
    - disk manage
    - linux
---

安装linux时，往往最先遇到的就是`disk`的分区挂载处理，这是一个很重要的部分之一。因为操作不当可能导致我们存在的数据丢失(双系统)，所以操作的每一步都需要谨慎。闲话少说，直接进入下面的正题。

{% colorquote info %}
os: --(pop_os!)
env: --(?)
tips: 
1) 不常用的命令`man`一下，`man`知道十万个为什么
2) 好博客不如烂操作，动手尝试一下
3) 试不过扇
{% endcolorquote %}

首先，我们来明确以下我们的目标：

disk ------> space ------> home for data

1) 我怎么知道磁盘在哪？以及磁盘是否能够分出空间？

`fdisk,lsblk`: `disk`不会主动告诉你，但是你可以问我们

```shell
$ sudo fdisk -l
# 这里所有字段字面意思
Disk /dev/nvme0n1: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
...

$ lsblk -f 
NAME FSTYPE FSVER LABEL UUID FSAVAIL FSUSE% MOUNTPOINTS
...

```

2) 现在知道了磁盘在哪，以及是否可以分出空间，怎么个分法？

`fdisk`: 你好，我们又见面了

```shell
$ sudo fdisk <disk-name>
...
#[fdisk]>交互式操作工具，按照提示来即可

```

3) 分完了，那数据怎么知道它应该在哪？

`mkfs,mount`: 疑似有点急段了

```shell
$ sudo mkfs.[fs_type] [disk_partition]

$ sudo mount [disk_partition] [mount_point]

```

{% colorquote warning %}
格式化指定文件系统和特殊挂载点可以查看相关安装文档的详细介绍
{% endcolorquote %}

4) 事不过三，至此就完成了`disk`的所有操作。

#### 后话

到这里相信肯定有人默默地在想：你这什么都没有，就一些简单的命令我遇到不会的那怎么办？这个其实很好办：不会就查资料，没有人天生就会所有东西，这里给出的是一个大致的思路，而不是一个教程，因为教程需要自己不断地实践。


