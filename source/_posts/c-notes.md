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
> 		std::ios_base::out | std::ios_base::app/ate/trunc 这些组合的方式来操作文件，如果指定的路径该文件不存在就会创建一个空的.
>
> 2. 如果std::ios_base::in 和 std::ios_base_out同时使用就会按照 std::ios_base_in,也就是即使文件不存在也不会创建.
> 3. std::ios_base::in | std::ios_base::out | std::ios_base::app/ate/trunc 这些组合的方式来操作文件, 如果指定路径的文件不存在也不会创建而且会把当前stream的state设置为std::ios_base::badbit.
> 4. 由于std::ios_base::out和std::ios_base::trunc单独使用的时候在打开已存在文件的情况下都会清空文件内容，因此我们使用的时候要格外注意.例如：我们可以通过std::ios_base::out | std::ios_base::app组合的形式来消除打开已存在文件时候的清空动作.

### 3. gdb调试

- gdb输出map元素大小写法

```shell
auto it = map_features.begin();
it.operator*().second.size();
//那么在gdb中若输出map_features元素大小需要这么做
> p map_features.begin().operators*().second.size()
```

- gdb在文件中设置断点

  ```shell
  break filename:linenum
  如下面的这行命令会在main.cpp中的281行设置断点
  > b main.cpp:281
  ```


### 4.将c中的char数组转换为整数类型

```c
char myarray[5] = {'-', '1', '2', '3', '\0'};
int i;
sscanf(myarray, "%d", &i);
```

参考：[Convert char array to a int number in C](https://stackoverflow.com/questions/10204471/convert-char-array-to-a-int-number-in-c?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)

### 5.实现c++ vector的二分查找

```c++
bool binary_search(const vector<string>& sorted_vec, string key) {
    size_t mid, left = 0 ;
    size_t right = sorted_vec.size(); // one position passed the right end
    while (left < right) {
        mid = left + (right - left)/2;
        if (key > sorted_vec[mid]){
            left = mid+1;
        }
        else if (key < sorted_vec[mid]){                                        
            right = mid;
        }
        else {                                                                  
            return true;
        }                                                                                                               
    }

    return false;      
}
```

参见：[Using Binary Search with Vectors](https://stackoverflow.com/questions/18774858/using-binary-search-with-vectors?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)

​		