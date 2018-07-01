---
title: shell常用语法
date: 2018-06-07 10:14:05
tags: [shell, linux]
categories: shell
---

linux中必会的语法

<!--more-->

## 1. shell如何设置默认参数

```shell
$ cat > test.sh
if [ ! $1 ]; then fortest=1; else fortest=$1; fi
echo $fortest

$ sh test.sh
1

$ sh test.sh 0
0
```

参考：[shell默认参数](https://blog.csdn.net/lsjseu/article/details/51492278)

## 2. 数组的声明与定义

```shell
#!/bin/bash
#符号``之间的命令执行后将会直接成为数组，xargs表示将标准输入转换为命令行参数传给basename,而basename可以提取.bam后缀的文件名前缀，即如果`ls *[1-9].bam`输出的是“1.bam,2.bam”，那么管道“|”后面的命令将输出"1 2",然后用括号括起来就成了一个数组
array=(`ls *[1-9].bam | xargs basename -s .bam`)

for sampleName in ${array[@]} #在for循环中遍历
do
	echo sampleName
done
```

参考：[How to initialize a bash array with output piped from another command?](https://stackoverflow.com/questions/971162/how-to-initialize-a-bash-array-with-output-piped-from-another-command?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)

## 3. shell判断文件或文件夹是否存在

```shell
if [ -d $filename ]; then echo "True"; fi #-d 判断文件夹是否存在
if [ -f $filename ]; then echo "True"; fi #-f 判断文件是否存在
if [ ! -f $filename ]; then echo "False"; fi #判断文件是否不存在
```



