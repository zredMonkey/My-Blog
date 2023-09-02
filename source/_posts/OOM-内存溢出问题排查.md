---
title: OOM-内存溢出问题排查
tags:
  - OOM
  - 内存溢出
categories:
  - OOM
  - 内存溢出
top_img: /img/oom.png
cover: /img/oom.png
keywords: OOM
description: >-
  内存溢出指程序申请内存时，没有足够的内存供申请者使用，或者说，给了你一块存储int类型数据的存储空间，但是你却存储long类型的数据，那么结果就是内存不够用，此时就会报错OOM,即所谓的内存溢出
post_meta:
  page:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
  post:
    date_type: both
    date_format: relative
    categories: true
    tags: true
    label: true
abbrlink: a67acf23
date: 2020-05-11 20:50:18
---
# **OOM(内存溢出的问题)排查**

- **内存泄漏(Memory Leak)**:是指程序在申请内存后,无法释放已申请的内存空间,一次内存泄漏似乎不会有大的影响,但内存泄漏堆积后的后果就是内存溢出。

- **内存溢出(Memory Overflow)**：指程序申请内存时，没有足够的内存供申请者使用，或者说，给了你一块存储int类型数据的存储空间，但是你却存储long类型的数据，那么结果就是内存不够用，此时就会报错OOM,即所谓的内存溢出。


**OOM原因：**

1.一次性申请的数量太多
(更改申请对象数量)

2.内存资源耗尽未释放
(找到未释放的对象进行释放)

3.本身资源不够
jmap -heap 查看堆信息


**OOM如何定位和解决的方法**
**1.系统已经OOM挂了**

提前设置：-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=
```
java -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath= -jar xxxx.jar
```
![图片](/img/oom-1.png)
再利用Java VisualVM去分析这个文件。

**2.系统运行中还未OOM**

导出dump文件：

```
jmap -dump:format=b,file=./jvm_logs/xushu.hprof 24286
```


注：
(1) file=表示dump文件打印的位置。
(2)24286表示进程号。


或使用Arthas软件。


---

线上系统OOM排查步骤：
1. 使用top命令看一下线上的CPU、内存使用情况。
2. 定位异常进程，看日志。
3. 如果是OOM，则用jstat -gc pid 1000 100(每秒)监控JVM内存运行情况和gc频率。
4. jmap -dump:live,format=b,file=dump3.hprof pid,
   使用jmap dump内存快照。(导出dump文件命令)
5. 使用MAT工具分析。
可以看下面的注。

注：
1. jps (查看进程号)
2. jmap -histo:live 进程号 > 存放路径  (将进程打印到文本中)
3. start 路径名 (打开文本，查看实例个数)
4. ps -ef | grep 服务名 ps -aux | grep 服务名
   查看服务的进程是否存在
5. 查看服务的日志
   cat -n xxx_log |grep "OutOfMemoryError"
6. 查看堆内存占用概况
   jmap -heap 进程号
7. 查看堆中对象的统计信息
   jmap -histo 进程号 | head -n 100
8. 查看GC统计信息
   jstat -gcutil 进程号
```
   S0 S1 E O M CCS YGC YGCT FGC FGCT GCT
   0.00 0.00 100.00 99.94 90.56 87.86 875 9.307 3223 5313.139 5322.446
```
    S0：幸存1区当前使用比例
    S1：幸存2区当前使用比例
    E：Eden Space（伊甸园）区使用比例
    O：Old Gen（老年代）使用比例
    M：元数据区使用比例
    CCS：压缩使用比例
    YGC：年轻代垃圾回收次数
    FGC：老年代垃圾回收次数
    FGCT：老年代垃圾回收消耗时间
    GCT：垃圾回收消耗总时间
    
9. **生产对堆快照Heap dump**

   **jmap -dump:format=b,file=/tmp/进程号_jmap_dump.hprof 进程号**


