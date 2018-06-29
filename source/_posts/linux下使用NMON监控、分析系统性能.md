---
title: linux下使用NMON监控、分析系统性能
date: 2018-06-29 09:10:47
tags: [nmon, 性能监控, linux]
categories: nmon
---

## 1. 下载nmon(nmon16e_mpginc.tar.gz)

根据CPU的类型选择下载相应的版本：
http://nmon.sourceforge.net/pmwiki.php?n=Site.Download

## 2. 初始化nmon工具

```shell
$ tar –zxvf nmon16e_mpginc.tar.gz
#生成各个平台的相应nmon版本
#eg. nmon_x86_64_ubuntu15
#根据不同的平台，初始化对应平台的nmon工具：
$ chmod +777 nmon_x86_64_ubuntu15
$ cp nmon_x86_64_ubuntu15 /usr/local/bin/nmon
```

## 3. 运行nmon工具

```shell
$ nmon
#按c显示各种性能数据， q退出
```

## 4. 生成nmon报告

````shell
#1.采集数据
$ nmon -s10 -c60 -f -m /home/
#10s采集一次，共采集60次(即采集10分钟)， -f 生成的数据文件名中包含文件创建的时间。-m 生成的数据文件的存放目录。
#2.生成报表
#下载 nmon analyser (生成性能报告的免费工具)（nmon_analyser_v52_1.zip，在windows解压使用）：将之前生成的 nmon 数据文件传到 Windows 机器上，用 Excel 打开分析工具 nmon analyser v46C.xls 。点击 Excel 文件中的 "Analyze nmon data" 按钮，选择 nmon 数据文件，这样就会生成一个分析后的结果文件： hostname_150924_1306.nmon.xls ，用 Excel 打开生成的文件就可以看到结果了。
````

