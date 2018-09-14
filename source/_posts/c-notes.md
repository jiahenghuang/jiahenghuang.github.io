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

### 2. c++ ios::base用法

> 1. 凡包含std::ios_base::out mode的操作方式，如果文件不存在都是会创建.
>
>    std::ios_base::out | std::ios_base::app/ate/trunc 这些组合的方式来操作文件，如果指定的路径该文件不存在就会创建一个空的.
>
> 2. 如果std::ios_base::in 和 std::ios_base_out同时使用就会按照 std::ios_base_in,也就是即使文件不存在也不会创建.
>
> 3. std::ios_base::in | std::ios_base::out | std::ios_base::app/ate/trunc 这些组合的方式来操作文件, 如果指定路径的文件不存在也不会创建而且会把当前stream的state设置为std::ios_base::badbit.
>
> 4. 由于std::ios_base::out和std::ios_base::trunc单独使用的时候在打开已存在文件的情况下都会清空文件内容，因此我们使用的时候要格外注意.例如：我们可以通过std::ios_base::out | std::ios_base::app组合的形式来消除打开已存在文件时候的清空动作.

```c
//追加并写入文件到本地将openmode修改为ios::app | ios::out,下面的实现是采用了默认参数，默认覆盖写入
string fcout(const string & fn, const string & fc, const std::ios::openmode & F_W = (ios::out | ios::trunc)) //默认覆盖写入
{
    ofstream if_(fn.c_str(), F_W);
    if (!if_.is_open()) {
        cout << "- make sure the file path is accessible!" << endl;
        assert(if_.is_open());
    }

    if_ << fc;
    if_.close();

    return fn;
}	
```

<!--more-->

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

### 5.vector操作

- 实现c++ vector的二分查找（参见：[Using Binary Search with Vectors](https://stackoverflow.com/questions/18774858/using-binary-search-with-vectors?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)）

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

- c++ 对vector元素求和

```c
int arr[]={10,20,30,40,50};  
vector<int> va(&arr[0],&arr[5]);  
int suum=accumulate(va.begin(),va.end(),0);
```

参考：[vector容器中所有元素求和-accmulate](https://blog.csdn.net/u011484045/article/details/43347475)

- vector的连接

```c
//如果在a后面插入b，则方法为
a.insert(a.end(),b,begin(),b.end());
//如果在a的前面插入b，则方法为
a.insert(a.begin(),b,begin(),b.end());
```

参考:[两个vector相连接--vector.insert用法](https://blog.csdn.net/Xiaohei00000/article/details/50799277)

- 反转(reverse)一个vector(https://stackoverflow.com/questions/8877448/how-do-i-reverse-a-c-vector)

```c
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> a;
    std::reverse(a.begin(), a.end());
    return 0;
}
```

### 6. c/c++ cpu_time和wall_time实现

```c
#include <iostream>

using namespace std;

int main(){
    auto get_wall_time = []{
        struct timeval time;
        if (gettimeofday(&time,NULL)){
            //  Handle error
            return 0.;
        }
        return (double)time.tv_sec + (double)time.tv_usec * .000001;
    };

    auto get_cpu_time = []{
        return (double)clock() / CLOCKS_PER_SEC;
    };

    //  Start Timers
    double wall0 = get_wall_time();
    double cpu0  = get_cpu_time();

    //  Perform some computation.
    double sum = 0;
    #pragma omp parallel for reduction(+ : sum)
    for (long long i = 1; i < 10; i++){
        sleep(1);
        sum += log((double)i);
    }

    //  Stop timers
    double wall1 = get_wall_time();
    double cpu1  = get_cpu_time();

    cout << "Wall Time = " << wall1 - wall0 << endl;
    cout << "CPU Time  = " << cpu1  - cpu0  << endl;

    //  Prevent Code Elimination
    cout << endl;
    cout << "Sum = " << sum << endl;
}
```

### 7. 判断是否为无穷

```c
#include<math.h>
isfinite(x)
```

### 8. m个字符的n元素的组合-基于位移实现

```c
//如输入为碱基A、C、G、T，输出为"AA", "AC", "AG", "AT"。。。即相当于依次在两个盒子中分别有放回的取出一个字符放在两个盒子中。共有2^4种可能，将每一种可能都看成对应的二进制的，2个bit存储一个碱基，于是2^4种可能对应着0-(2^4-1)的2^4种二进制表示。再将A,C,G,T的vector的索引看成二进制，于是得到的2个bit就可以用来取出vector中对应的碱基。
int main(){
	auto Combination=[](vector<string> &bases, int & numComb)
    {
        int length = bases.size();
        int numBitsToRepresent = ceil(length / 2.0);
        int combineTotal = pow(2, numComb * numBitsToRepresent);
        int baseMask = pow(2, numBitsToRepresent) - 1;
        vector<string> allCombine; allCombine.resize(pow(length, numComb));
        int ind = 0;
        for (int combineCase = 0; combineCase < combineTotal; combineCase++)
        {
            string numCombBases = "";
            bool skip = false;
            for (int i = numComb - 1; i >= 0; i--)
            {
                int base_ind = (combineCase >> (numBitsToRepresent * i) & baseMask);
                //                int base_ind = (combineCase & (baseMask << (numBitsToRepresent * i))) >> (numBitsToRepresent * i);
                if (base_ind < length)
                {
                    numCombBases += bases[base_ind];
                }
                else
                {
                    skip = true;
                    break;
                }
            }
            if (!skip)
            {
                allCombine[ind] = numCombBases;
                ind++;
            }
        }
        return allCombine;
    };

    vector<string> bases{"A", "C", "G", "T", "R"}; //唯一的元素，可重复

    Combination(bases, 3);
}
```

### 9. 在c语言中调用系统命令判断文件是否存在并删除文件

```c
if( access( "/mnt/hgfs/VM_Share/c_read.txt", F_OK ) != -1 )
{
    string rm_c_read = string("rm ") + "/mnt/hgfs/VM_Share/c_read.txt";
	system(rm_c_read.c_str());
}
//use R_OK, W_OK, and X_OK in place of F_OK to check for read permission, write permission, and execute permission (respectively) rather than existence, and you can OR any of them together (i.e. check for both read and write permission using R_OK|W_OK)
//Note that on Windows, you can't use W_OK to reliably test for write permission, since the access function does not take DACLs into account. access( fname, W_OK ) may return 0 (success) because the file does not have the read-only attribute set, but you still may not have permission to write to the file.
```

### 10.int转无符号整数取绝对值

```c++
#include <limits.h>
#include <stdio.h>
#include <stdlib.h>

// This solves the issue of using the standard abs() function
unsigned int absu(int value) {
	return (value < 0) ? -((unsigned int)value) : (unsigned int)value;
}

int main(void) {
    printf("INT_MAX: %d\n", INT_MAX);
    printf("INT_MIN: %d\n", INT_MIN);
    printf("absu(INT_MIN): %u\n", absu(INT_MIN));

    return 0;
}
```

### 11.c/c++在linux上的环境准备

- clion安装与卸载(http://www.voidcn.com/article/p-evedqfrg-kd.html)

```shell
$ tar -zxvf CLion-2016.3.5.tar.gz#解压
#进入bin目录,执行sh文件：
$ ./clion.sh
$ sudo notepadqq /etc/hosts##添加0.0.0.0 account.jetbrains.com进hosts文件
#添加激活码：http://idea.lanyus.com/
```

**卸载**

> Delete the installation directory
> Delete the "config" and "system" configuration directories. These contain IntelliJ IDEA's caches, configuration and plugins.`（~/.<PRODUCT><VERSION>）`

- cmake下载安装

```shell
#第一种办法,如果不行，试试第二种
sudo apt-get install cmake

#第二种办法
#In the new version of cmake (ex: 3.9.6), to install, download tar from https://cmake.org/download/. Extract the downloaded tar file and then:

cd $CMAKE_DOWNLOAD_PATH
./configure
make
make install
```

参见：

[ubuntu安装CMake的几种方式 ](https://blog.csdn.net/lj402159806/article/details/76408597)

https://askubuntu.com/questions/829310/how-to-upgrade-cmake-in-ubuntu/829311

- g++环境

  ```shell
  #g++环境
  sudo apt-get install g++
  #fatal error: bzlib.h: No such file or directory 问题
  sudo apt-get install libboost-all-dev
  sudo apt-get install libbz2-dev
  #fatal error: zlib.h & lzma.h : No such file or directory
  sudo apt-get install liblzma-dev
  ```

  参考：

  [g++: command not found的解决 ](https://blog.csdn.net/h378588270/article/details/7729268)

  [解决 Boost安装：fatal error: bzlib.h: No such file or directory 问题](https://www.cnblogs.com/qq952693358/p/8563048.html)

  [fatal error: zlib.h & lzma.h : No such file or directory ](https://blog.csdn.net/digent1/article/details/9467739)











		