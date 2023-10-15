---
title: BigDecimal(double)和BigDecimal(String)的区别
tags:
  - BigDecimal(double)
  - BigDecimal(String)
categories:
  - BigDecimal(String)
  - BigDecimal(double)
top_img: /img/bigdecimal.jpg
cover: /img/bigdecimal.jpg
keywords: BigDecimal
description: BigDecimal(double)和BigDecimal(String)的区别
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
abbrlink: ce3886d7
date: 2023-10-15 19:30:18
---
先说答案，BigDecimal(double)和BigDecimal(String)有区别，而且区别很大。

因为double是不精确的，所以使用一个不精确的数字来创建BigDecimal，得到的数字也不是精确的。如0.1这个数字，double只能表示它的近似值。

所以，当我们使用BigDecimal(0.1)创建一个BigDecimal的时候，其实创建出来的值并不是正好等于0.1的。

而是0.1.00000000000055511151231257827021181583404541015625。这是因为double自身表示的只是一个近似值。

而对于BigDecimal(String)，当我们使用new BigDecimal("0.1")创建一个BigDecimal的时候，其实创建出来的值正好就是等于0.1的。


那么它的标度就是1。


## 扩展知识
在《阿里巴巴Java开发手册》有一条建议，或者要求是说：
![图片](/img/decimal-wenzhang-1.png)



未完待续。。。
