---
title: awk与sed使用技巧
date: 2018-05-10 20:00:33
tags: [awk, sed, linux, 工作效率]
categories: linux
---

1. awk按条件输出文本的某些行

```shell
$ less noFilter.realn.sam | awk '{if ($3 == "chr16") {print $0}}' >> chr16.sam
```

<!--more-->

2. sed替换文件中的某一行

```shell
#对103行进行修改
sed -i '103c #define FLAG_USE_MULTI_THREAD 0 //thr_,singlethread <= 0' main.cpp
$ cat > ab
#123
#456
$ sed -i '1c hi' ab
$ cat ab
#hi 
#456
#注：i参数是指直接修改读取的文件内容，而不是由萤幕输出。若不加，则只会在屏幕输出，而不会修改文件。1c hi是指第一行修改为hi
```

​