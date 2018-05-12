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

2. 