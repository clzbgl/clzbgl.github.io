---
layout: post
title:  "Linux磁盘空间查看及空间满的处理"
date:   2019-09-24 10:08:42 +0800
categories: tech
typora-root-url: ..
---

查看磁盘空间还剩多少空间

```shell
df -h
```

查看根目录下每个目录占用空间大小

```shell
du --max-depth=1 -h  /
```

查看当前目录占用空间大小

```shell
du -sh 
```

当前目录下的文件按大小排序

```shell
# du
du -s ./* | sort -nr | head -10

# ls实现列文件按时间排序
ls -lhSr 按大小排序，大的显示在最后
ls -lt 时间最近的在前面
ls -ltr 时间从前到后

# 利用sort
ls -l | sort +7 (日期为第8列) 时间从前到后
ls -l | sort -r +7  时间最近的在前面
```

删除文件

```shell
find -size 0 -exec rm '{}' ';'   删除文件大小为0的文件
```

