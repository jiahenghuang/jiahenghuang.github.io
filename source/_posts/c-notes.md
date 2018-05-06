---
title: c++ notes
date: 2018-05-06 15:59:06
tags: [c++, notes]
categories: c++
---

## vs调试时如何显示输出窗口

在代码中加上如下预处理命令

```c++
#define __P__  system("pause");return 0;
__P__ （放到main里面）
```

