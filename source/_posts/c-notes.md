---
title: c++ notes
date: 2018-05-06 15:59:06
tags: [c++, notes]
categories: c++
---

## 1. vs调试时如何显示输出窗口

在代码中加上如下预处理命令

```c++
#define __P__  system("pause");return 0;
__P__ （放到main里面）
```

<!--more-->

### 2. c++ ios::base用法

> 1. 凡包含std::ios_base::out mode的操作方式，如果文件不存在都是会创建.
>
> ​	std::ios_base::out | std::ios_base::app/ate/trunc 这些组合的方式来操作文件，如果指定的路径该文件不存在就会创建一个空的.
>
> 2. 如果std::ios_base::in 和 std::ios_base_out同时使用就会按照 std::ios_base_in,也就是即使文件不存在也不会创建.
> 3. std::ios_base::in | std::ios_base::out | std::ios_base::app/ate/trunc 这些组合的方式来操作文件, 如果指定路径的文件不存在也不会创建而且会把当前stream的state设置为std::ios_base::badbit.
> 4. 由于std::ios_base::out和std::ios_base::trunc单独使用的时候在打开已存在文件的情况下都会清空文件内容，因此我们使用的时候要格外注意.例如：我们可以通过std::ios_base::out | std::ios_base::app组合的形式来消除打开已存在文件时候的清空动作.

### 3. gdb调试

```c++
//gdb输出map元素大小写法
auto it = map_features.begin();
it.operator*().second.size();
```

```shell
#那么在gdb中若输出map_features元素大小需要这么做
> p map_features.begin().operators*().second.size()
```




​		