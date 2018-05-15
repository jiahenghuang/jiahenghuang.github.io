---
title: 云服务器ssh防超时设置
date: 2018-05-13 16:09:43
tags: [linux, 云服务器, 防超时]
categories: linux
---

## 云服务器使用时如何防超时

如果你在windows下通过工具连接，可以设置

- secureCRT：

  选项---session---终端---反空闲 中设置每隔多少秒发送一个字符串，或者是NO-OP协议包

- putty：

  putty -> Connection -> Seconds between keepalives ( 0 to turn off ), 默认为0, 改为300.